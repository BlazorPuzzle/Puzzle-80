# Puzzle #80 - Hidden JavaScript Issue

Help Jeff and Carl figure out why their simple app is failing, and not offering much help.

YouTube Video: https://youtu.be/w447J952lT8

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

This is a .NET 9 Blazor Web App with Global WebAssembly Interactivity

This code is as simple as it gets for JavaScript Interop in Blazor.

After loading, the app fails with an error that's virtually impossible to debug.

What's going on here, and how do we fix it?

## The Solution

The problem is that the proc is called "name", a label which is already used in Blazor.

One fix is to simply rename it to something unique, like we did here.

A better fix is to scope the JavaScript to this component.

You can see how that is done by creating a new Razor Class Library project.

```c#
<script>
    window.nameBlazorPuzzle = () => {
        const answer = prompt("What is your name?");
        alert("Hello " + answer + "!");
    };
</script>

@code {
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await _jsRuntime.InvokeVoidAsync("nameBlazorPuzzle");
        }
    }
}
```

Boom!
