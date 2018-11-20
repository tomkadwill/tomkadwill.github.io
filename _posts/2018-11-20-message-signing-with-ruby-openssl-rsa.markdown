---
layout:     post
title:      "How to sign messages with Ruby's OpenSSL RSA algorithm"
date:       2018-11-20 18:00:00 +0000
permalink:  'message-signing-with-ruby-openssl-rsa'
---

In this post I’ll talk about signing and verifying messages. Imagine that I’m building a chat application. Messages will be sent via a pub/sub system like [Pusher](https://pusher.com/) or [Pubnub](https://www.pubnub.com/). Here is a simple JSON message object:

```json
{
  "user_id": "123",
  "message": "Hello world!"
}
```

The problem with this implementation is that any user can be impersonated by changing the `user_id`. To solve this problem, each user can sign their messages with a key that only they poses. One way to build this is to use RSA, an asymmetric cryptographic algorithm. Asymmetric algorithms generate two keys: one public and one private. The private key is used for signing messages and the public key is used for verifying messages. Here is how to write it in Ruby:

First, each user needs a public and private key:

```ruby
rsa_key = OpenSSL::PKey::RSA.new(2048)

private_key_pem = rsa_key.to_pem #=> "chat_public_key": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqd9paLELcSsdMA....\n-----END PUBLIC KEY-----\n,
public_key_pem = rsa_key.public_key.to_pem #=> "chat_private_key": "-----BEGIN RSA PRIVATE KEY-----\nMIIEogaY4wKPUV31NCZ\nYrJs+g47/zzHV5GYx5/Wv4zRhDyTi95....\n-----END RSA PRIVATE KEY-----\n,
```

Once keys have been generated, a message can be signed using the private key.

```ruby
private_key = OpenSSL::PKey::RSA.new(private_key_pem)

signature = private_key.sign(OpenSSL::Digest::SHA256.new, 'Hello world!') #=> "\x04\xEC\xCC?\xDE\x8F\x91>G\..."
```

After the signature has been generated it can be sent along with the message.

```json
{
  "user_id": "123",
  "message": "Hello world!",
  "signature": "\x04\xEC\xCC?\xDE\x8F\x91>G\..."
}
```

A user can verify any message signature using the senders public key.

```ruby
public_key = OpenSSL::PKey::RSA.new(public_key_pem)

public_key.verify(OpenSSL::Digest::SHA256.new, signature, message) #=> true
```

If the decoded signature matches the message then `#verify` will return true. If not, the user’s public key does not match the one used to sign the message.

Of course, there must be a mechanism for distributing public keys to all users. In the case of a chat application, there could be an API that returns a list of participants with their public keys.
