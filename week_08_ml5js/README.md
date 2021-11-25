# ml5.js - Friendly Machine Learning for the Web

> A lot of this will be reworked from the official [ML5.js documentation](https://learn.ml5js.org/#/) so I recommend you have a flick round there. But I'm just gonna cover some of the practical stuff about setting up a project and playing with some demos.

(In a nutshell) [ML5](https://learn.ml5js.org/) is a wrapper around [TensorflowJS](https://www.tensorflow.org/js) which aims to make working with machine learning _on the web_ much easier. TensorflowJS runs models which have been re-compiled to run in [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API) [shader programs](https://thebookofshaders.com/) which run on the GPU. Machine learning code can get complicated pretty quickly but ML5 aims to make that all simpler by offering a simpler [API](https://en.wikipedia.org/wiki/API) to work with the models and handle the input and output of data. It's also very easy to set up...

---

## Setup

There is some [boilerplate](https://en.wikipedia.org/wiki/Boilerplate_code) in the [ML5 GitHub repo](https://github.com/ml5js/ml5-library/tree/main/examples/javascript/ml5Boilerplate/ml5Boilerplate_Version) which I've also put in this repo (`/00_setup`). You can simply import ML5 from a [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) straight into a simple HTML document, and then start writing JavaScript code. A basic setup looks like this:

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Getting Started with ml5.js</title>
    <!-- ml5 -->
    <script src="https://unpkg.com/ml5@latest/dist/ml5.min.js"></script>
  </head>

  <body>
    <script src="sketch.js"></script>
  </body>
</html>
```

```javascript
// Your code will go here

// Open up your console - if everything loaded properly you should see the version number 
// corresponding to the latest version of ml5 printed to the console and on the screen.
console.log('ml5 version:', ml5.version);

document.body.append(document.createTextNode(ml5.version));
```

## Local Server

In order to run these locally on your machine you'll need to be using a [local server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server). There are various ways of doing this but I'd recommend using the simple and very popular [http-server](https://www.npmjs.com/package/http-server) which can be installed via [NPM](https://www.npmjs.com/package/http-server). You could also use the Python based [http.server](https://docs.python.org/3/library/http.server.html) (scroll to the bottom and you will see the commands for running this from your terminal/command-prompt.

Here's Dan Shiffman talking in more depth about setting up a local server:

[![Coding Train: Local Server](https://img.youtube.com/vi/UCHzlUiDD10/0.jpg)](https://www.youtube.com/watch?v=UCHzlUiDD10)

## Image Classification

Take a look in `/01_image_classification` and have a look at the code for setting up a simple [MobileNet](https://arxiv.org/pdf/1704.04861.pdf) classifier. ML5 code uses a lot of [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which is JavaScripts way of handling [asynchronous programming](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Concepts) - not really something you need to worry about at this stage - but essentially the code might look a bit weird. When you see blocks like this:

```javascript
ml5.imageClassifier('MobileNet')
  .then(classifier => classifier.classify(image))
  .then(results => {
    console.log(results)
  });
```

This is a chain of Promises where the initial call is to load the model `ml5.imageClassifier('MobileNet')`, the output of this function is piped into the next where we use the model to classify the image `.then(classifier => classifier.classify(image))`, then we cosole log the results `.then(results => { console.log(results) });`.. Can take a little while to get used to this syntax but think of it quite literally where we do one thing __then__ take the results and do something else, __then__ do something else in a long chain.

> NB: This is only applicable to Promises in JavaScript, you can't use the `.then()` syntax wherever you want unfortunately.

There are two versions of the JavaScript code in `/01_image_classification`. `sketch_1.js` uses the longer chain spoken about above. This isn't really that useful as the model isn't stored any where for continual use. `sketch_2.js` saves the model to a variable so you could load it once and then continue to use it (attach the function call to a button or something to be able to make a user interface of some kind.

## PoseNet

I'm gonna be honest, this works for me but is _painfully slow_. I think something is up with ML5 here as other models work fine. But let me know how it works for you...

## Pix2Pix

This is a nice example of turning a common model into a a fun little drawing tool. It is the most complex one yet, but worth picking through the code as it is a good example how how to initialise the code (load the model and set up global variables and such), handle mouse interaction and create a stateful interactive web page.
