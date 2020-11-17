# Zoho CRM Widgets
This repository is meant to be a pragmatic intro to building Widgets in Zoho CRM. Zoho CRM Widgets allow you to create a small web app embedded in Zoho CRM using JavaScript. To be able to build meaningful widgets you will need basic knowledge of HTML, CSS, and JavaScript (including Promises). 

Here are some JavaScript resources that are pretty good:
- [Free Code Camp YouTube Tutorial](https://www.youtube.com/watch?v=PkZNo7MFNFg)
- [Eloquent JavaScript](https://eloquentjavascript.net/)
- [W3 Schools JavaScript Reference](https://www.w3schools.com/jsref/default.asp)

There's an absurd amount of JavaScript content on the internet, so just find what works for you.

## Why CRM Widgets?
CRM Widgets game-changers when you need additional functionality in Zoho CRM, but Deluge functions and workflow rules don't cut it. Some examples of this are mass updates of records with a Lookup field or mass updating related records. These are perfect use cases for widgets.

In the following section we will cover how you can rapidly setup your first Zoho CRM widget project.

## Setting Up a Coding Environment
If you are new to programming outside of the Zoho ecoystem, welcome! We're going to cover the most direct path for getting started with a widget project, and you can learn along the way. We will provide some links to helpful HTML, CSS, and JavaScript resources if you need them.

If you are familiar with JavaScript and Node.js, you can skip the next couple sections and pick back up with the **Install Zoho CLI** section.

### Install a Code Editor
When programming outside of Deluge, you use a code editor or IDE. These are desktop applications that help you write your code. I recommend using [Visual Studio Code](https://code.visualstudio.com/), but there are other popular choices for beginners such as [Atom](https://atom.io/) and [Sublime](https://www.sublimetext.com/). It doesn't matter too much, you take your pick.

Visit the linked website to download and install the program of your choice.

### Install Node.js and NPM
#### Node.js? NPM?
First off, we will define Node.js and NPM in just a second. For now let's get into JavaScript a little more. JavaScript is a programming language that let's you write code that is executed by your web browser. Since it manipulates the user interface of a website, it is called a front-end programming language.

That takes us to Node.js. Node.js is essentially just JavaScript packaged up to be executed as a web server or another app to be run by a computer. This means Node.js is a backend programming language. Node.js makes your life easier by only having to learn one programming language to build both the front-end and backend of your project. To build a CRM widget you do not *need* to write your own Node.js backend, but simply execute existing Node.js code.

NPM (Node Package Manager) is a command line tool for downloading and installing JavaScript and Node.js packages. You don't need to worry to much about this now, but you will need to download it.

#### Installation
Visit https://nodejs.org/en/download/ and download either the Windows installer or Mac installer, depending on your system. If you're using Linux you probably don't need someone to explain what to do :). Once downloaded, open the program and follow the prompts. All the default options should be fine for our purposes.

NPM comes with Node.js, so you won't need to worry about that. Before we move on, let's check everything installed properly.

Open your terminal and execute the following:

```
$ node -v
```

Which should output something like: `v12.18.4`

Then execute:

  ```
  $ npm -v
  ```

Which should output something like: `6.14.6`

Everything should now be good to go with Node.js and NPM.


### Install the Zoho CLI
Zoho has created a CLI (Command Line Interface) for creating CRM Widgets and Extensions. It will give a pre-configured setup to run and build your project. You can view more info about the Zoho CLI [here](https://www.zoho.com/crm/developer/docs/widgets/install-cli.html).

To install the CLI run the following command in your terminal:

  ```
  $ npm install -g zoho-extension-toolkit
  ```

When it is done installing, test it worked properly by executing the following in your terminal:

  ```
  $ zet
  ```

You should get a response similar to: 
```
Usage: zet [options] [command]

Options:
  -v, --version  Shows the version number
  -h, --help     output usage information

Commands:
  init           Creates a new extension template directory
  run            Starts a local server with current directory as context
  validate       Validates the current app with validation rules
  pack           Packs the project to upload into marketplace
  ```
You should now be ready to get started with your project.

## Setting Up Your Widget Project
### Initialize Your Project
Through the Zoho CLI, you can very quickly start a project. Open your terminal and navigate to the directory you want to create a new project within. Run the following command in your terminal:

  ```
  $ zet init
  ```
  
 You will now see an interface for entering your project's information. Input the following for the input:

```
Select the Zoho service for your widget and hit enter key
  Zoho CRM

Project Name
  * YOUR PROJECT NAME *
```

Your project should now be setup. Now navigate to your project's directory with
 
 ```
 $ cd * YOUR PROJECT NAME *
 ```

### Run Your App
Now that you're in your project's directory, let's start our project in the browser. Execute the following:

  ```
  $ zet run
  ```
  
Now navigate to [https://127.0.0.1:5000/widget.html](https://127.0.0.1:5000/widget.html) in your browser. You will probably get a security screen saying this is unsafe. If you are using Google Chrome, click anywhere on the screen and type "thisisunsafe" and it will let you through.

### Adding the Zoho JavaScript SDK
Zoho has also created a SDK (Software Development Kit) for developing Widgets. This SDK allows you to very easily use the Zoho CRM API. You can view the Zoho CRM JS SDK [here](https://help.zwidgets.com/help/latest/index.html).

To add this package to your project, you will need to include it into your HTML via a CDN. Unfortunately it is not currently an NPM package.

Open up your code editor and open up `app/widget.html`. We're now going to include the Zoho JS SDK in our project. Paste the following in the `<head>` of your HTML:

  ```
  <script src="https://live.zwidgets.com/js-sdk/1.0.5/ZohoEmbededAppSDK.min.js"></script>
  ```

The head should now look something like this:
```
  <head>
    <meta charset="UTF-8">
    <script src="https://live.zwidgets.com/js-sdk/1.0.5/ZohoEmbededAppSDK.min.js"></script>
  </head>
```

Within the same directory as your `widget.html` file, create two more directories titled `js` and `css`. This will contain your JavaScript and CSS, respectively. In the `js` directory, create a file called `app.js` (or whatever you want). Now add the following line *below* the Zoho JS SDK in your HTML file:

  ```<script src="js/app.js"></script>```


You now need to initialize your JavaScript application with Zoho CRM. You do that by inserting the following into your `app.js` file:

```
  // Get Entity Data from CRM (Note: this only works within Zoho CRM)
  ZOHO.embeddedApp.on("PageLoad", entity => {
      // This is the information about the current record, if applicable.
      console.log(entity);
  });

  // Initialize Widget Connection
  ZOHO.embeddedApp.init();
```

Now that your have the right setup, let's get our project installed in Zoho CRM.

### Embedding Your Project in Zoho CRM
To get our project to start running within Zoho CRM we need to setup two things: Widgets and Buttons.

#### Creating a New Widget
Go to *Setup* -> *Developer Space / Widgets* and click *Create New Widget*.

Add the following configurations to your project:
```
Name: *YOUR WIDGET NAME*
Widget Type: Button
Hosting: External
Base URL: https://127.0.0.1:5000/app/widget.html
```

This setup will pull in the widget running on your local web server into Zoho CRM. Since this is the case, it will only work when you are on your machine while your app is running. Because of this, we will only do this during development. When we put the widget into production you will upload a zip file of your project.

#### Creating a New Button
Go to *Setup* -> *Customization / Modules and Fields*, then select the module you want to host the button. Go to the *Links and Buttons* tab and select *New Button*. Add the following configuration to setup your button:

```
Name: *YOUR BUTTON NAME*
Where would you like to place the button?
  View Page
  
What action would you like the button to perform?
  Open a Widget
  Install your new widget.
```

Click save. Now navigate to the module you put the button on and select a record.

Open the Developer Tools in your browser to be able to see the console. In Chrome, right-click and select *Inspect*.

With the console visible, click the newly created button. You should see the entity information in your console. It will be structured similar to the following:

```
{EntityId: Array(1), Entity: "Contacts", ButtonPosition: "DetailView"}
```


### Build the Widget
You are now setup to build the widget of your dreams. We won't cover how to build a JavaScript applicaton, so you're on your own there. Here are some things to note about building a CRM Widget:
- To use the Zoho CRM API, you will use functions from the Zoho JS SDK we included early. You can see the JS API reference [here](https://help.zwidgets.com/help/latest/index.html).
- You can use any JavaScript or CSS framework you would like within your widget.


### Uploading the Production Widget
When you are done with development, it is time to upload your final version as a zip file to Zoho CRM. To zip your widget, navigate to your project in the terminal and execute:
```
  $ zet pack
```

This will pack your project into a zip file that you can upload into Zoho CRM. You can find this zip file in the `dist` directory inside your widget's directory. 

Go to your widget (*Setup* -> *Developer Space / Widgets*) and click the *Settings* icon, then *Edit*.

Change the *Hosting* to *Zoho* and upload your zip file in *File Upload*. Your *Index Page* (unless you have moved the widget.html file) will be `/widget.html`. 

Any user can now use your new widget. If it is not working, delete the Button and create a new one, then install the widget in that button.
