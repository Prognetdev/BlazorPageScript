# Page script component
PageScript component for loading external JS files while using SSR with enhanced navigation and forms.

Based on https://learn.microsoft.com/en-us/aspnet/core/blazor/javascript-interoperability/static-server-rendering?view=aspnetcore-8.0

## Usage
In your razor SSR component: _PageWithScript.razor_
```html
@using Prognetdev.BlazorPageScript

<PageScript Src="./Components/Pages/PageWithScript.razor.js" />
```
Place your JS file in the same folder as the razor component: _PageWithScript.razor.js_
```javascript
export function onLoad() {
    console.log('Loaded');
}

export function onUpdate() {
    console.log('Updated');
}

export function onDispose() {
    console.log('Disposed');
}
```

## What is it
This component is used to load external JS files when using SSR with enhanced navigation and forms. 
It will load the JS file when the component is rendered and dispose of it when the component is removed.
This is useful if you want to load a JS file for a specific component and not for the entire application.

Maybe you want to initialize rich text editor or a date picker for a specific component without using
webassembly or interactive serverside rendering with websocket connection in the background.

## SSR and enhanced navigation
When using SSR with enhanced navigation, the component will be rendered on the server and will be seamlessly
replaced with blazor script `<script src="_framework/blazor.web.js"></script>` creating SPA-like experience. 
This creates a problem for scripts that need to be executed on page load. This RCL is responsible for executing
initialization logic of such scripts. By using pattern from Microsoft documentation, we can load the script using
exported functions above.

### Per component execution
From Microsoft documentation:
> To reuse the same module among pages, but have the onLoad and onDispose callbacks invoked on each page change, 
> append a query string to the end of the script so that it's recognized as a different module. 
> An app could adopt the convention of using the component's name as the query string value. 
> In the following example, the query string is "counter" because this PageScript component 
> reference is placed in a Counter component. This is merely a suggestion, 
> and you can use whatever query string scheme that you prefer.
```html
<PageScript Src="./Components/Pages/PageWithScript.razor.js?counter" />
```