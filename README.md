# Fetch-to-Flow

This project is a simple proof-of-concept to demonstrate POSTing a JSON payload,
using the [JavaScript Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), to a Request Trigger URL in MS PowerAutomate.

## Prerequisites

- A copy of this repository (or at least the `index.html` file)
- An Office365 account licenses to use PowerAutomate. 
- A working NodeJS environment (preferred) or a web server to host the `index.html` file.

## Setting Up

### 1. Create the PowerAutomate Flow

First, use your web browser to navigate to the [Office 365 PowerAutomate Web API](https://flow.microsoft.com/) and create a new **Instant Cloud Flow**.
Give your new flow a name, and where asked how to trigger the flow, find and select `When an HTTP request is received` from the list.
Click **Create**.

Next, open the trigger's dialog and copy-and-paste the following JSON code into the `Request Body JSON Schema` input:

```
{
    "properties": {
        "topic": {
            "type": "string"
        },
        "email": {
            "type": "string"
        },
        "name": {
            "type": "string"
        },
        "source": {
            "type": "string"
        }
    },
    "type": "object"
}
```

Click the trigger's **Show advanced options** and then set the **method** to `POST`.

Now **Insert a new step** into the workflow. Search for `email` and choose `Send an email notification (V3)`.
Add your own email address in the **To:** field and provide a **Subject** like _Form submission from Fetch-to-Flow_ tool.
You could even use **Dynamic content** to replace _Fetch-to-Flow_ with the `name` parameter parsed from the JSON payload.

Click in the **Body** and use **Dynamic content** to put the JSON payload's parameters into an email.
This might look something like:  

> You submitted the form. Here's what you told us:
> 
> Your name: [`name`]  
> Your email: [`email`]  
> A thing you like: [`topic`]  
> 
> The form was submitted using the [`source`] tool.

Save your flow and then copy the `HTTP POST URL` from the trigger.

### 2. Configure the HTML Form 

Open the `index.html` file in this repository in your text editor.
Find the `<script>` tag at the bottom of the page and update the value of the `myFlowURL` constant with the URL you copied from your flow&#39;s trigger.

### 3. Using the Form

If you cloned this repository to your NodeJS environment, install it (`$ npm install`) and then run `$ npm run serve`.
Visit <http://127.0.0.1:8080> in your browser, then fill and and submit the form.

If you don't have a working NodeJS environment, you can also simply put your `index.html` file on a web server (preferably a development or other private environment) and access it there.

After submitting the form, if your Flow is working as intended, you'll receive an email with the submitted form values.

## Notes

The idea for this proof-of-concept was inpired by [this article](https://powerusers.microsoft.com/t5/Power-Automate-Community-Blog/Use-Microsoft-Flow-to-Integrate-Your-Website-s-Forms-with/ba-p/45206)

