<div align="center">

# `Phoenix LiveView` Realtime Cursor Tracking Tutorial

![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial/ci.yml?label=build&style=flat-square&branch=main)
[![codecov.io](https://img.shields.io/codecov/c/github/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial/main.svg?style=flat-square)](https://codecov.io/github/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial?branch=main)
[![Hex.pm](https://img.shields.io/hexpm/v/phoenix?color=brightgreen&style=flat-square)](https://hex.pm/packages/phoenix)
[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat-square)](https://github.com/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial/issues)
[![HitCount](https://hits.dwyl.com/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial.svg?style=flat-square&show=unique)](https://hits.dwyl.com/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial)

Learn how to create a **live cursor tracking app** using **Phoenix LiveView**!

</div>

<br />

- [`Phoenix LiveView` Realtime Cursor Tracking Tutorial](#phoenix-liveview-realtime-cursor-tracking-tutorial)
- [Why? 🤷](#why-)
- [What? 💭](#what-)
- [Who? 👤](#who-)
    - [Prerequisites](#prerequisites)
- [How? 💻](#how-)
  - [Step 0: Run the finished app locally](#step-0-run-the-finished-app-locally)
    - [Clone the repo](#clone-the-repo)
    - [Download the Dependencies](#download-the-dependencies)
    - [Run the App](#run-the-app)
- [Build It!](#build-it)
  - [1. Create the App 🆕](#1-create-the-app-)
    - [1a. Setting up `Tailwind CSS`](#1a-setting-up-tailwind-css)
      - [Installing TailwindCSS](#installing-tailwindcss)
    - [Setup watcher, minification and loading Tailwind](#setup-watcher-minification-and-loading-tailwind)
    - [1b. Running the Phoenix app](#1b-running-the-phoenix-app)
  - [2. LiveView with cursors 🔺](#2-liveview-with-cursors-)
    - [2a. Tracking cursor movement](#2a-tracking-cursor-movement)
  - [3. Adding users :adult:](#3-adding-users-adult)
    - [3a. Adding usernames](#3a-adding-usernames)
    - [3b. Tracking who is online](#3b-tracking-who-is-online)
  - [4. Customization 🎨](#4-customization-)
    - [4a. Different colors](#4a-different-colors)
  - [5. Adding authentication 🔐](#5-adding-authentication-)
    - [5a. Add a login button](#5a-add-a-login-button)
    - [5b. Add `auth_plug`](#5b-add-auth_plug)
    - [5c. Showing username](#5c-showing-username)
- [Credits 📝](#credits-)

<br />

# Why? 🤷

As we're building our 
[MVP](https://github.com/dwyl/mvp),
we are always trying to leverage the _awesome_ real-time
features of `Phoenix` and `LiveView`. 

Live cursor tracking will enable _us_ to create a
team-friendly environment 
in which `people` can _see_
what the others are clicking on
when collaborating in a prioritization 
(e.g. backlog grooming) exercise.

# What? 💭

This tutorial creates a stylized simple landing page
showing the cursor of everyone that is connected
to it. 
It should take you **20 minutes**
to follow from start to finish.

# Who? 👤

This tutorial is aimed at `LiveView` beginners 
who want to understand the ins and outs, 
while sparkling a bit of 
[`TailwindCSS`](https://tailwindcss.com/)
magic :sparkles:.

If you are completely new to `Phoenix` and `LiveView`,
we recommend you follow the **`LiveView` _Counter_ Tutorial**:
[dwyl/phoenix-liveview-counter-tutorial](https://github.com/dwyl/phoenix-liveview-counter-tutorial)

### Prerequisites 

This tutorial requires you have `Elixir` and `Phoenix` installed.
If you you don't, please see 
[how to install Elixir](https://github.com/dwyl/learn-elixir#installation)
and 
[Phoenix](https://hexdocs.pm/phoenix/installation.html#phoenix).

# How? 💻

## Step 0: Run the finished app locally

To get a feel of what you are going to build and
to be _make sure_ it's working in 1 minute, do the
following steps:

### Clone the repo

Run the following commands to clone the repo.

```
git clone https://github.com/dwyl/phoenix-liveview-realtime-cursor-tracking-tutorial.git
cd phoenix-liveview-realtime-cursor-tracking-tutorial
```

### Download the Dependencies

Install the dependencies by running the command:

```sh
mix setup
```

This will download dependencies 
and compile your files. 
It might take a few
minutes!

### Run the App

Start the Phoenix server by running the command:

```sh
mix phx.server
```

Now you can visit
[`localhost:4000`](http://localhost:4000)
in your web browser.

> 💡 Open a _second_ browser window or tab
>  (**in incognito mode**, if you want to),
> and you will see the a new cursor with a new name!


You should expect to see something similar to the following:

![cursor-tracking-demo-GIF](https://user-images.githubusercontent.com/194400/197960714-a82e945c-2511-4ff3-bbd1-47ed751ac3ce.gif)


# Build It! 

Now that you've seen the app working,
it's time to _build_ it from scratch!


## 1. Create the App 🆕

In your terminal run the following `mix` command
to generate the new Phoenix app:

```sh
mix phx.new live_cursors --no-ecto --no-mailer --no-dashboard --no-gettext
```

This command will create a new `Phoenix` app 
and setup the dependencies 
(including the `LiveView` dependencies)
The flags: 
`--no-ecto --no-mailer --no-dashboard --no-gettext`
just mean we don't want a database, email, dashboard or translation.
These are not needed in a simple demo/experiment app.

When you see the following prompt in your terminal, 
press **`Y` to install the dependencies**.

```sh
Fetch and install dependencies? [Yn]
```

Type <kbd>Y</kbd> followed by the <kbd>Enter</kbd> key.
That will download all the necessary dependencies.

### 1a. Setting up `Tailwind CSS`

We are going to be using `Tailwind` to style our page. 
If this is the first time using `Tailwind`, 
see:
[dwyl/**learn-tailwind**](https://github.com/dwyl/learn-tailwind)
for a primer. 

The reason we are using TailwindCSS is to showcase you
how to also integrate a *mature* styling library and
get it running with Phoenix so you can create your 
own awesome and beautiful web pages :smile:.

#### Installing TailwindCSS
Let's first add the TailwindCSS dependency to our project,
by opening the `mix.exs` file and adding
the following line to the `deps` section:

```elixir
{:tailwind, "~> 0.1.9", runtime: Mix.env() == :dev},
```

Next, in the `config/config.exs` file, 
let's specify the Tailwind version we are 
going to be using.

```elixir
config :tailwind,
  version: "3.2.0",
  default: [
      args: ~w(
      --config=tailwind.config.js
      --input=css/app.css
      --output=../priv/static/assets/app.css
      ),
      cd: Path.expand("../assets", __DIR__)
  ]
```

Now, simply run the following command to install
the dependency:


```sh
mix deps.get
```

And then run the following Tailwind tasks. These 
will download the standalone Tailwind CLI and 
generate a `tailwind.config.js` file
in the `./assets` directory.

```sh
mix tailwind.install
mix tailwind default
```

### Setup watcher, minification and loading Tailwind

Throughout development we want Tailwind CLI 
to track our files for changes. For this, add
the following watcher in `config/dev.exs` file.

```elixir
  watchers: [
    //...
    tailwind: {Tailwind, :install_and_run, [:default, ~w(--watch)]},
  ]
```

After developing our application, we want tailwind to 
minify our CSS files in production. For this to happen,
update the following line in the `mix.exs` file.

```elixir
  defp aliases do
    [
      setup: ["deps.get"],
      "assets.deploy": ["esbuild default --minify", "tailwind default --minify", "phx.digest"]
    ]
  end
```

All we have to do now is make sure tailwind classes
are loaded in our `assets/css/app.css`. The tasks
you ran previously probably already took care of this,
but make sure you have these added to your file.

```elixir
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";

//@import "./phoenix.css";   -> deleted
```

Also make sure your `assets/js/app.js`
was changed as well. Make sure the 
`import "../css/app.css";` line is commented
or removed.

After all of this, you should be done! You can 
start styling your webpages with some sweet CSS :tada:.

### 1b. Running the Phoenix app

To make sure everything is working properly, 
run the following:

```sh
mix phx.server
```

If you visit [`localhost:4000`](http://localhost:4000), you'll
see the following in your browser.

<img width="1379" alt="phoenix_with_tailwind" src="https://user-images.githubusercontent.com/17494745/197478954-a4ce8e52-8a14-45c5-b80a-883639c5f1f6.png">

> Don't worry if this looks ugly. It's because
we now use TailwindCSS. We're going to make it
pretty in the next few minutes!

## 2. LiveView with cursors 🔺

Let's start by adding a LiveView to our application. 
Let's switch the current default route to a new 
`live` route in the `lib/live_cursors_web/router.ex` file.

```elixir
  scope "/", LiveCursorsWeb do
    pipe_through :browser
    live "/", Cursors
  end
```

Next, we create a `live` folder in `lib/live_cursors_web/live`,
and create a file called `cursors.ex` with the following code.
We are placing assigning `x` and `y` to the socket, 
which will refer to the mouse position within the page.

```elixir
defmodule LiveCursorsWeb.Cursors do
    use LiveCursorsWeb, :live_view

    def mount(_params, _session, socket) do
      {:ok, 
        socket
          |> assign(:x, 50)
          |> assign(:y, 50)
      }
    end
end
```

In the same folder, we create a `cursors.html.heex` file with the
follow code:

```
<ul class="list-none">
    <li style={"left: #{@x}%; top: #{@y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
      <svg xmlns="http://www.w3.org/2000/svg" width="31" height="32" fill="none" viewBox="0 0 31 32">
        <path fill="url(#a)" d="m.609 10.86 5.234 15.488c1.793 5.306 8.344 7.175 12.666 3.612l9.497-7.826c4.424-3.646 3.69-10.625-1.396-13.27L11.88 1.2C5.488-2.124-1.697 4.033.609 10.859Z"/>
        <defs>
          <linearGradient id="a" x1="-4.982" x2="23.447" y1="-8.607" y2="25.891" gradientUnits="userSpaceOnUse">
            <stop style={"stop-color: #5B8FA3"}/>
            <stop offset="1" stop-color="#BDACFF"/>
          </linearGradient>
        </defs>
      </svg>
    </li>
</ul>
```

> The reason we have the HTML template on a different file
instead of using the `render` function with the `~H` sigil
is purely for dev experience purposes. With VS Code, 
we can get TailwindCSS hints and code completion
which we wouldn't otherwise using a `render` function 
in the `cursors.ex` file.

Now delete all the code in the 
`lib/live_cursors_web/templates/page/index.html.heex` 
and change the `lib/live_cursors_web/templates/layout/root.html.heex` file. 
We just delete the header 
and render the content on our `cursors.html.heex` file.

```elixir
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <meta name="csrf-token" content={csrf_token_value()}>
    <%= live_title_tag assigns[:page_title] || "LiveCursors", suffix: " · Phoenix Framework" %>
    <link phx-track-static rel="stylesheet" href={Routes.static_path(@conn, "/assets/app.css")}/>
    <script defer phx-track-static type="text/javascript" src={Routes.static_path(@conn, "/assets/app.js")}></script>
  </head>
  <body>
    <%= @inner_content %>
  </body>
</html>
```

You should now see a cursor 
at the middle of the page.

<img width="1379" alt="simple_cursor_middle" src="https://user-images.githubusercontent.com/17494745/197489203-ce81beea-d30a-4aa2-948d-cb63635cae19.png">


> You can change your `svg` object to anything you like. 
> This custom cursor was created using [Figma](https://www.figma.com/).


### 2a. Tracking cursor movement

We are going to use Javascript 
to track the mouse movement on the client 
and send it over to the Phoenix server 
we are building. 

We understand that the idea of LiveView 
is to do as much work on the server as possible, 
but in this specific scenario, that is not possible.

Phoenix offers 
[options for client/server interoperability](https://hexdocs.pm/phoenix_live_view/js-interop.html), 
including **client hooks**. 
We are using the `phx-hook` binding for this. 
We are going to send events from Javascript 
to the server through this binding. 

In this case, we are using the `mousemove` 
event to send the updated coordinates to 
the server. 
For this, add the following code in the `assets/js/app.js`.

```javascript
let Hooks = {};

Hooks.TrackClientCursor = {
  mounted() {
    document.addEventListener('mousemove', (e) => {
      const mouse_x = (e.pageX / window.innerWidth) * 100; // in %
      const mouse_y = (e.pageY / window.innerHeight) * 100; // in %
      this.pushEvent('cursor-move', { mouse_x, mouse_y });
    });
  }
};

let csrfToken = document.querySelector("meta[name='csrf-token']").getAttribute("content")
let liveSocket = new LiveSocket("/live", Socket, {hooks: Hooks, params: {_csrf_token: csrfToken}})
```

In the previous code block, 
we track the mouse position within the page in percentage. 
This way, the mouse position 
is displayed correctly in any device. 

The `mousemove` event handler 
is added to the `Hook` object which,
 in turn, is passed through the `liveSocket`.

After this, we need to *handle the event* in the
`cursors.ex` file. Let's add the event handler with
the following code:

```elixir
  def handle_event("cursor-move", %{"mouse_x" => x, "mouse_y" => y}, socket) do
    {:noreply,
      socket
        |> assign(:x, x)
        |> assign(:y, y)
    }
  end
```

In the `cursors.html.heex` file, 
let's change the first two lines with the
following piece of code:

```
<ul class="list-none mt-9 max-h-full max-w-full" id="cursor" phx-hook="TrackClientCursor">
    <li style={"left: #{@x}%; top: #{@y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
```

Here, we are using the `phx-hook` binding 
with the hook created in the `app.js` file.
Make sure to ann an `id` to the element so that LiveView 
can properly identify it.

If you restart the server and open the browser again,
the cursor should follow your mouse movements.

<img width="1379" alt="cursor_tracking" src="https://user-images.githubusercontent.com/17494745/197502086-f3a0ff05-5604-41ce-ad4f-3a72b05d8414.png">


## 3. Adding users :adult:
We have our own mouse cursor moving. We need to 
*broadcast* this movement information to every user
that joins the channel. Additionally, we need to 
identify who in the channel so we show their respective
cursor with their name.

In our application, whenever a user visits the website,
a session will be created and we'll assign a random
username to the user who joined. 

### 3a. Adding usernames

To generate random names, we'll be using the 
[mnemonic_slugs](https://github.com/devshane/mnemonic_slugs) 
library. To do this, let's install it. 
Add the following line to the `mix.exs` file.

```elixir
{:mnemonic_slugs, "~> 0.0.3"},
```

and run the command below to install it.

```sh
mix deps.get
```

We then change our `cursors.ex` file to 
*create* a new username to the person joining
the page and add it to the socket assigns.
Your `mount/3` function should now look
like this.

```elixir
  def mount(_params, _session, socket) do

    username = MnemonicSlugs.generate_slug

    {:ok,
      socket
        |> assign(:x, 50)
        |> assign(:y, 50)
        |> assign(:username, username)
    }
  end
```

Now let's show this username! Head to
`cursors.html.heex` and let's add a simple
`<span>` with the `@username` assign we just 
added to the socket. The file should look
like this.

```elixir
<ul class="list-none mt-9 max-h-full max-w-full" id="cursor" phx-hook="TrackClientCursor">
    <li style={"left: #{@x}%; top: #{@y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
      <svg xmlns="http://www.w3.org/2000/svg" width="31" height="32" fill="none" viewBox="0 0 31 32">
        <path fill="url(#a)" d="m.609 10.86 5.234 15.488c1.793 5.306 8.344 7.175 12.666 3.612l9.497-7.826c4.424-3.646 3.69-10.625-1.396-13.27L11.88 1.2C5.488-2.124-1.697 4.033.609 10.859Z"/>
        <defs>
          <linearGradient id="a" x1="-4.982" x2="23.447" y1="-8.607" y2="25.891" gradientUnits="userSpaceOnUse">
            <stop style={"stop-color: #5B8FA3"}/>
            <stop offset="1" stop-color="#BDACFF"/>
          </linearGradient>
        </defs>
      </svg>
          
      <span class="mt-1 ml-4 px-1 text-sm text-white rounded-xl bg-slate-600">
        <%= @username %>
      </span>
    </li>
</ul>
```


If you restart the server and open the browser, 
you will see the cursor and a random username
moving along with it.

<img width="1379" alt="cursor_with_username" src="https://user-images.githubusercontent.com/17494745/197506546-2208c9ee-6f36-4625-b49b-126eef60d629.png">

### 3b. Tracking who is online

This is the most exciting part of the tutorial. 
We are going to have different users have their 
cursors and see them in real-time! For this, we
need to be able to track *who is online in
the channel*. For this, we are going to be
using **Phoenix Presence**. We will use Presence
to store information about the *coordinates* and *username* in the Presence channel. 

> Phoenix Presence is a package within the Phoenix
ecosystem that allows us to track processes and users
that are within a channel. It provides many features.
We recommend reading [their documentation](https://hexdocs.pm/phoenix/Phoenix.Presence.html)
if you are interested in learning more.

Let's start by adding PResence
to our application. Run the following command.

```sh
mix phx.gen.presence
```

After installing, the terminal should ask you to 
follow some instructions. 

```sh
Add your new module to your supervision tree,
in lib/live_cursors/application.ex:

    children = [
      ...
      LiveCursorsWeb.Presence
    ]
```

Let's follow the instructions and do 
exactly that. Open `lib/live_cursors/application.ex`
and add `LiveCursorWeb.Presence` to the `children` array.

With Presence installed, let's now work on 
the `cursors.ex` file. First, let's add 
`alias LiveCursorsWeb.Presence` 
after declaring the module so it's easier
to write our code.

In the `mount/3` function, we will initialize Presence
for the channel through the `Presence.track/4` function. 
We will define a constant `@channel_topic`
and use it in the function as well. Change the
code in `cursors.ex` file so the mounting function
looks like such:

```elixir
defmodule LiveCursorsWeb.Cursors do
  alias LiveCursorsWeb.Presence
  use LiveCursorsWeb, :live_view

  @channel_topic "cursor_page"

  def mount(_params, _session, socket) do

    username = MnemonicSlugs.generate_slug

    Presence.track(self(), @channel_topic, socket.id, %{
      socket_id: socket.id,
      x: 50,
      y: 50,
      username: username,
    })

    LiveCursorsWeb.Endpoint.subscribe(@channel_topic)

    initial_users =
      Presence.list(@channel_topic)
      |> Enum.map(fn {_, data} -> data[:metas] |> List.first() end)

    updated =
      socket
      |> assign(:username, username)
      |> assign(:users, initial_users)
      |> assign(:socket_id, socket.id)

    {:ok, updated}
  end
```

To further explain what we just wrote, 
we are telling Presence to track ourselves, within
the channel, the `id` of the socket and adding 
metadata we want to track, namely the `username` 
and the mouse coordinates (`x` and `y`). 
Additionally, whenever the user acecss the page, 
it **subscribes** to the channel events 
in the `LiveCursorsWeb.Endpoint.subscribe(@channel_topic)`. 
After this, we create an `initial_users` variable, 
where we use the data from Presence and obtain the list of
connected users and their respective metadata. 
At last, we assign the `user` data 
and the `initial_users` 
and `socket.id` data to the socket assigns, 
so we can access this information 
in the `cursors.html.heex` file.

Speaking of which, let's change the `cursors.html.heex` file
to reflect these changes. We are going to iterate over
the list of users that are online and render a cursor and name
for each one. Your file should now look like the following:

```elixir
<ul class="list-none mt-9 max-h-full max-w-full" id="cursor" phx-hook="TrackClientCursor">
  <%= for user <- @users do %>
    <li style={"left: #{user.x}%; top: #{user.y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
      <svg xmlns="http://www.w3.org/2000/svg" width="31" height="32" fill="none" viewBox="0 0 31 32">
        <path fill="url(#a)" d="m.609 10.86 5.234 15.488c1.793 5.306 8.344 7.175 12.666 3.612l9.497-7.826c4.424-3.646 3.69-10.625-1.396-13.27L11.88 1.2C5.488-2.124-1.697 4.033.609 10.859Z"/>
        <defs>
          <linearGradient id="a" x1="-4.982" x2="23.447" y1="-8.607" y2="25.891" gradientUnits="userSpaceOnUse">
            <stop style={"stop-color: #5B8FA3"}/>
            <stop offset="1" stop-color="#BDACFF"/>
          </linearGradient>
        </defs>
      </svg>
      <span class="mt-1 ml-4 px-1 text-sm text-white rounded-xl bg-slate-600">
        <%= user.username %>
      </span>
    </li>
  <% end %>
</ul>
```

After this, we ought to handle the `cursor-move` event coming 
from the client and update the coordinates in the list of
active users in the channel with the `socket.id` as an identifier.
Change the `handle_event/1` function to this.

```elixir
  def handle_event("cursor-move", %{"mouse_x" => x, "mouse_y" => y}, socket) do
    key = socket.id
    payload = %{x: x, y: y}

    metas =
      Presence.get_by_key(@channel_topic, key)[:metas]
      |> List.first()
      |> Map.merge(payload)

    Presence.update(self(), @channel_topic, key, metas)

    {:noreply, socket}
  end
```

The last thing that is left is to make our LiveView *react*
to whenever a user joins or leaves the channel. 
Therefore, we sync the socket with the info available
in the Presence channel. The following function 
**updates the list of active users** whenever someone
joins or leaves the channel.

```elixir
  def handle_info(%{event: "presence_diff", payload: _payload}, socket) do
    users =
      Presence.list(@channel_topic)
      |> Enum.map(fn {_, data} -> data[:metas] |> List.first() end)

    updated =
      socket
      |> assign(users: users)
      |> assign(socket_id: socket.id)

    {:noreply, updated}
  end
```

That was a lot! Now let's restart the server
and open two different tabs. You will see the cursor
moving in both tabs **in real-time**. Awesome stuff!

<img width="1379" alt="multiple_users" src="https://user-images.githubusercontent.com/17494745/197518449-482a0286-f711-4890-93a4-deb0c9f7aabd.png">


## 4. Customization 🎨

Now that we got the main feature working, let's customize
our page so it looks better! 🎨

Let's add a background to the page. 
We used [heropatterns.com](https://heropatterns.com/)
to create our background pattern. 
You can customize the background to your liking 
and then head to `assets/css/app.cs` and create a class
`.bg-pattern` and paste the pattern contents. 

Now, let's change the `cursors.html.heex` to add a text
and add the background. CHange the file so it looks like this:

```
<div class="bg-pattern flex justify-center items-center h-screen w-screen bg-[#eafbfa]">
  <div class="absolute pointer-events-none">
    <p class="text-[4rem] font-extrabold text-transparent bg-clip-text bg-gradient-to-br from-[#a7edff] to-[#ff6565] opacity-20">just use your mouse</p>
  </div>
  <ul class="list-none mt-9 max-h-full max-w-full" id="cursor" phx-hook="TrackClientCursor">
    <%= for user <- @users do %>
      <li style={"left: #{user.x}%; top: #{user.y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
        <svg xmlns="http://www.w3.org/2000/svg" width="31" height="32" fill="none" viewBox="0 0 31 32">
          <path fill="url(#a)" d="m.609 10.86 5.234 15.488c1.793 5.306 8.344 7.175 12.666 3.612l9.497-7.826c4.424-3.646 3.69-10.625-1.396-13.27L11.88 1.2C5.488-2.124-1.697 4.033.609 10.859Z"/>
          <defs>
            <linearGradient id="a" x1="-4.982" x2="23.447" y1="-8.607" y2="25.891" gradientUnits="userSpaceOnUse">
              <stop style={"stop-color: #5B8FA3"}/>
              <stop offset="1" stop-color="#BDACFF"/>
            </linearGradient>
          </defs>
        </svg>
        <span class="mt-1 ml-4 px-1 text-sm text-white rounded-xl bg-slate-600">
          <%= user.username %>
        </span>
      </li>
    <% end %>
  </ul>
</div>
```

Notice the `bg-pattern` class being used in the first line.

After restarting the server, your webpage should look like this:

<img width="1379" alt="styling_page" src="https://user-images.githubusercontent.com/17494745/197522991-d69eeefb-3641-4862-8181-9f3991742f02.png">

### 4a. Different colors
Let's add some finishing touches. Each user will have its own 
color associated. First, let's add the 
[`random_color`](https://hexdocs.pm/random_color/RandomColor.html)
library.

```elixir
{:random_color, "~> 0.1.0"}
```

Now, in the `cursors.ex` file, the same way we generate
a random username everytime a user joins and add it to the 
socket assigns, let's do the same for the color. 
Make the necessary changes to the file so the `mount/3` function
looks like this.

```elixir
  def mount(_params, _session, socket) do

    username = MnemonicSlugs.generate_slug
    color = RandomColor.hex()

    Presence.track(self(), @channel_topic, socket.id, %{
      socket_id: socket.id,
      x: 50,
      y: 50,
      username: username,
      color: color
    })
```

Now, all that's left for us to do
is changing the `cursors.html.heex` to get the color and 
show it to the client. Change the `<span>` element
of the username and change the background color
according to the `color` assign. It should look look this:

```
<span style={"background-color: #{user.color};"} class="mt-1 ml-4 px-1 text-sm text-white rounded-xl">
  <%= user.username %>
</span>
```

And you should see a different color under each user's username.

<img width="1379" alt="different_colors" src="https://user-images.githubusercontent.com/17494745/197528509-121cad50-b4ae-49e4-9784-356ea8e9a2ae.png">


If you want to remove the cursor pointer and just have the `svg`
element as your cursor, simply add the following piece of code
to the `assets/css/app.css` file:

```css
.body {
  cursor: none
}
```

## 5. Adding authentication 🔐

Right now, the app assigns a random name
to the user once he starts using the app.
However, it would be interesting 
for the user to **login** using 
a given provider (such as Google or Github)
and show the related username for everyone to see!

For this, we can leverage the 
[`auth_plug`](https://github.com/dwyl/auth_plug)
package to easily integrate this feature.

Let's do this!

### 5a. Add a login button

We should add a way for the user to login.
Let's add a button to the canvas.
In the `lib/live_cursors_web/live/cursors.html.heex` file,
change it so it looks like the following.

```html
<div class="bg-pattern flex justify-center items-center h-screen w-screen bg-[#eafbfa]">
  <div class="absolute pointer-events-none">
    <p class="text-[4rem] font-extrabold text-transparent bg-clip-text bg-gradient-to-br from-[#a7edff] to-[#ff6565] opacity-20 text-center">just use your mouse</p>
  </div>


  <input
    id="auth-btn"
    type="submit"
    phx-click="login"
    class="absolute right-6 bottom-6 flex-shrink-0
     text-white bg-gradient-to-r to-[#b6c5ff] from-[#e07db3]
     text-base font-semibold py-2 px-4 rounded-lg shadow-xl
     hover:bg-pink-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-pink-200"
    value="Login"
  />

  <ul class="list-none mt-9 max-h-full max-w-full" id="cursor" phx-hook="TrackClientCursor">
    <%= for user <- @users do %>
      <li style={"left: #{user.x}%; top: #{user.y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
        <svg xmlns="http://www.w3.org/2000/svg" width="31" height="32" fill="none" viewBox="0 0 31 32">
          <path fill="url(#a)" d="m.609 10.86 5.234 15.488c1.793 5.306 8.344 7.175 12.666 3.612l9.497-7.826c4.424-3.646 3.69-10.625-1.396-13.27L11.88 1.2C5.488-2.124-1.697 4.033.609 10.859Z"/>
          <defs>
            <linearGradient id="a" x1="-4.982" x2="23.447" y1="-8.607" y2="25.891" gradientUnits="userSpaceOnUse">
              <stop style={"stop-color: #{user.color}"}/>
              <stop offset="1" stop-color="#BDACFF"/>
            </linearGradient>
          </defs>
        </svg>
        <span style={"background-color: #{user.color};"} class="mt-1 ml-4 px-1 text-sm text-white rounded-xl">
          <%= user.username %>
        </span>
      </li>
    <% end %>
  </ul>
</div>
```

We've simply added a button that,
once clicked, creates a `login` event.
We are going to eventually handle this event,
but before that,
we need to add `auth_plug` to our project.

### 5b. Add `auth_plug`

Let's get this up and running fast!
Inside the `mix.exs` file,
add the following inside the `deps` section.

```elixir
{:auth_plug, "~> 1.5"},
```

and run 

```sh
mix deps.get
```

Afterwards, visit https://authdemo.fly.dev/apps/new,
sign in using a preferred provider 
and create an app for the `localhost:4000` URL,
the same URL you are running the app from.

![flyapp](https://user-images.githubusercontent.com/17494745/203396767-a73a6c5e-f484-49a2-9ece-225e5b09f740.png)

After creating an app, 
you should be prompted with an image similar
to the following.

![token](https://user-images.githubusercontent.com/194400/202419365-e06c43d6-c537-4646-a1a3-90320fb3aa59.png)

You now are going to run the app with this `AUTH_API_KEY`
environment variable. 
To run the app with this environment variable,

```sh
export AUTH_API_KEY=2cfxNaMmkvwKmHncbYAL58mLZMs/2cfxNa4RnU12gYYSwPvp2hSPFdVDcbdK/authdemo.fly.dev

mix phx.server
```

Now, let's create the `AuthController`.
This controller will be used to login 
and logout the user in the app.

Create a file 
`lib/live_cursors_web/controllers/auth_controller.ex`
and add the following.

```elixir
defmodule LiveCursorsWeb.AuthController do
  use LiveCursorsWeb, :controller
  import Phoenix.LiveView, only: [assign_new: 3]

  def add_assigns(:default, _params, %{"jwt" => jwt} = _session, socket) do
    {:cont, AuthPlug.assign_jwt_to_socket(socket, &assign_new/3, jwt)}
  end

  def add_assigns(:default, _params, _session, socket) do
    {:cont, assign_new(socket, :loggedin, fn -> false end)}
  end

  def login(conn, _params) do
    redirect(conn, external: AuthPlug.get_auth_url(conn, "/"))
  end

  def logout(conn, _params) do
    conn
    |> AuthPlug.logout()
    |> put_status(302)
    |> redirect(to: "/")
  end
end
```

The `add_assigns/3` functions will be used
to alter the socket with auth assigns. 
We will be using a `loggedin` and `person` assign,
where the latter has information about
the user that is logged in.

Lastly, open the `router.ex` file.
We are going to create an [*Optional Path*](https://github.com/dwyl/auth_plug#optional-auth)
pipeline, which will allow users to access a route, 
even if they are not logged in.

Change the code so it looks like the following.

```elixir
  pipeline :authOptional, do: plug(AuthPlugOptional)

  scope "/", LiveCursorsWeb do
    pipe_through :browser
    pipe_through :protect_from_forgery
    pipe_through :authOptional

    live "/", Cursors
    get "/login", AuthController, :login
    get "/logout", AuthController, :logout
  end
```

And you should be sorted!
If you had any trouble setting `auth_plug` running,
[check their docs](https://github.com/dwyl/phoenix-chat-example/blob/add-auth/auth.md),
as the guide is more thorough there.

### 5c. Showing username

Let's make the necessary changes so,
after logging in, the username of the user
is shown next to the cursor.

Firstly, let's handle the `login` event
we defined earlier.

In the `lib/live_cursors_web/live/cursors.ex` file,
add the following handler.

```elixir
  def handle_event("login", _value, socket) do
    {:noreply, push_redirect(socket, to: "/login")}
  end

  def handle_event("logout", _value, socket) do
    {:noreply, push_redirect(socket, to: "/logout")}
  end
```

The `login` will redirect the user to the `/login` URL path,
which is handled by the `AuthController`.
The same thing happens to the `logout` event.

Now let's change the UI.
Remember the button we added in the UI previously?
Let's change it. 
We are going to display a different button
depending on whether the user is logged in
or not.
Change `lib/live_cursors_web/live/cursors.html.heex`
so it looks like the following.

```html
<div class="bg-pattern flex justify-center items-center h-screen w-screen bg-[#eafbfa]">
  <div class="absolute pointer-events-none">
    <p class="text-[4rem] font-extrabold text-transparent bg-clip-text bg-gradient-to-br from-[#a7edff] to-[#ff6565] opacity-20 text-center">just use your mouse</p>
  </div>


  <%= if @loggedin do %>
    <input
      id="submit-msg-logout"
      type="submit"
      phx-click="logout"
      class="absolute right-6 bottom-6 flex-shrink-0
      text-white bg-gradient-to-r to-[#ffb6b6] from-[#e07db3]
      text-base font-semibold py-2 px-4 rounded-lg shadow-xl
      hover:bg-pink-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-pink-200"
      value="Logout"
    />
  <% else %>
    <input
      id="submit-msg-login"
      type="submit"
      phx-click="login"
      class="absolute right-6 bottom-6 flex-shrink-0
      text-white bg-gradient-to-r to-[#b6c5ff] from-[#e07db3]
      text-base font-semibold py-2 px-4 rounded-lg shadow-xl
      hover:bg-pink-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-pink-200"
      value="Login"
    />
  <% end %>

  <ul class="list-none mt-9 max-h-full max-w-full" id="cursor" phx-hook="TrackClientCursor">
    <%= for user <- @users do %>
      <li style={"left: #{user.x}%; top: #{user.y}%"} class="flex flex-col absolute pointer-events-none whitespace-nowrap overflow-hidden">
        <svg xmlns="http://www.w3.org/2000/svg" width="31" height="32" fill="none" viewBox="0 0 31 32">
          <path fill="url(#a)" d="m.609 10.86 5.234 15.488c1.793 5.306 8.344 7.175 12.666 3.612l9.497-7.826c4.424-3.646 3.69-10.625-1.396-13.27L11.88 1.2C5.488-2.124-1.697 4.033.609 10.859Z"/>
          <defs>
            <linearGradient id="a" x1="-4.982" x2="23.447" y1="-8.607" y2="25.891" gradientUnits="userSpaceOnUse">
              <stop style={"stop-color: #{user.color}"}/>
              <stop offset="1" stop-color="#BDACFF"/>
            </linearGradient>
          </defs>
        </svg>
        <span style={"background-color: #{user.color};"} class="mt-1 ml-4 px-1 text-sm text-white rounded-xl">
          <%= user.username %>
        </span>
      </li>
    <% end %>
  </ul>
</div>
```

We are using the `loggedin` socket assign 
to display a button saying `Login` or `Logout`.

The last thing to do is 
changing the username of the user!

In the `lib/live_cursors_web/live/cursors.ex` file,
change `mount/3` so it looks like this.

```elixir
  def mount(params, session, socket) do

    # Add auth assigns to socket
    {_cont, socket} = AuthController.add_assigns(:default, params, session, socket)

    username = if (socket.assigns.loggedin) do
      socket.assigns.person.username || socket.assigns.person.givenName || "guest"
    else
       MnemonicSlugs.generate_slug
    end

    color = RandomColor.hex()

    Presence.track(self(), @channel_topic, socket.id, %{
      socket_id: socket.id,
      x: 50,
      y: 50,
      username: username,
      color: color
    })

    LiveCursorsWeb.Endpoint.subscribe(@channel_topic)

    initial_users =
      Presence.list(@channel_topic)
      |> Enum.map(fn {_, data} -> data[:metas] |> List.first() end)

    updated =
      socket
      |> assign(:username, username)
      |> assign(:users, initial_users)
      |> assign(:socket_id, socket.id)

    {:ok, updated}
  end
```

We are calling the `add_assigns/4` method in `AuthController`
so it adds the proper auth socket assigns.
After adding these, 
we check if the user is logged in or not.
If he is, we use the `username` or `givenName` field.

And that's it! 
You should now see your username on the screen,
like so!

![auth_demo](https://user-images.githubusercontent.com/17494745/203618159-0f9ec94f-44b2-4fc9-b5e8-a2c5eb097c53.gif)

# Credits 📝

This tutorial was inspired by 
[Koen van Gilst's](https://koenvangilst.nl/blog/phoenix-liveview-cursors) 
walkthrough. 
Be sure to follow him: 
[**`@vnglst`**](https://github.com/vnglst)
