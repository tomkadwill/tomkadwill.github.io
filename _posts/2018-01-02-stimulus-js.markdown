---
layout:     post
title:      "Stimulus JS"
date:       2018-02-06 21:10:00 +0000
comments:   true
tags:       JS Rails
---

This blog post is about StimulusJS and whether your business should use it. I'll explain what Stimulus is and the problems that it solves. Lastly, I'll give my opinion about whether it's worth implementing Stimulus.

### What is Stimulus

A JavaScript framework, built by [basecamp](https://basecamp.com), designed to enhance HTML, rendered from the server. Stimulus is a front-end framework but it's different from frameworks like Angular and React. One of the key differences between Stimulus and other big-name front-end frameworks is that it's not concerned with rendering HTML. Here is an example to illustrate what this means.

I'm going to build a feature which allows a user to enter their name in an input field and receive a customised greeting. It's a silly feature but it will do for this explanation. To build the feature, I'm going to use Stimulus to render a Bootstrap modal, displaying the text that the user entered.

First I'm going add the HTML for the modal. I won't explain this code in detail as there are lots of tutorials online that explain Bootstrap modals.

```html
<div id="myModal" class="modal fade">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                <h4 class="modal-title"></h4>
            </div>
            <div class="modal-body">
                <p>This is your greeting</p>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            </div>
        </div>
    </div>
</div>
```

Next I'll add a simple div, input and button. To add stimulus I need to use the `data-controller`, `data-target` and `data-action` attributes.

`data-controller` tells Stimulus which JavaScript controller to use. In Stimulus, everything is done within a controller. In this case, Stimulus will use a controller called `hello_controller.js`.

`data-target` allows me to reference an element more easily. In this case, `data-target="hello.name"` allows me to access the input element, from `hello_controller.js`.

`data-action="click->hello#greet"` tells Stimulus, when a user clicks the button, run `greet()` inside `hello_controller.js`.


```html
<div data-controller="hello">
  <input data-target="hello.name" type="text">
  <button data-action="click->hello#greet">Greet</button>
</div>
```

Lastly, I need to write the controller. I'll create a file called `controllers/hello_controller.js`. Inside, I'll add the following code:

```javascript
import { Controller } from "stimulus"

export default class extends Controller {
  greet() {
    const element = this.targets.find("name");
    const name = element.value;

    $("#myModal").modal('show');
    $('h4.modal-title').text(`Hi ${name}`);
  }
}
```

As mentioned previously, `greet()` gets called when the user clicks the button. When the function is run it gets the value from the input field, renders the modal and then sets the modal text. Notice that I'm able to do `this.targets.find("name")`, Instead of having to find the input field via jQuery. That is because I set `data-target` on the input field.

### Should your business implement StimulusJS

Many Rails apps, that I've worked on, have poorly organised JavaScript code. Random functions are executed in `.erb` files, via includes and `$( document ).ready()`. It's often hard to track down what JavaScript functions are being run.

For these codebases, I think Stimulus is a big step forward. One that will increase the speed of development and reduce the number of JS bugs.

It's also important to re-iterate the importance of having Basecamp behind the project. This makes it a lot easier for anyone trying to make a business case for Stimulus. Especially given the short life span of most JavaScript frameworks. Basecamp has a history of long term support, I think this could be a competitive advantage.

Stimulus does have some drawbacks. For me, the biggest is testing. Many Rails applications have no JavaScript tests and Stimulus does not seem to fix this problem. I've seen no documentation about testing, however, I'm lead to believe that [testing is coming soon](https://github.com/stimulusjs/stimulus/issues/34).
