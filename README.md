# retroNode

retroNode is a tool built with Vue.js designed to simplify the creation of RPG text and graphic adventures. It features a visual scripting interface, making it straightforward for developers and storytellers to craft complex narrative paths. In retroNode, each node represents a unique room or scene. The platform offers drag-and-drop functionality for easy organization and detailed customization of scenes, enabling users to focus on creativity without getting bogged down in technical details.


https://github.com/doncabreraphone/retroNode/assets/45015326/ad6dcfb1-eae7-47a4-bf70-019129e8e1d9


## Features

- **Visual Scripting Interface**: Allows for intuitive organization of game scenes and rooms through a user-friendly drag-and-drop system.
- **Detailed Scene Customization**: Each node can be customized with images, visibility toggles, and navigational pathways (North, South, East, West) for immersive game experiences.
- **Interactive Scenario Canvas**: Integrates a canvas for rendering interactive scenarios, supporting custom visualization and scenario editing.
- **Control Panel for Quick Access**: Features a floating control panel with buttons for adding scenarios, exporting data, toggling drawing mode, and managing application state.
- **Export Functionality**: Enables exporting scenario data and generated code snippets for easy integration into game logic.

## Dependencies

retroNode leverages the power of Vue.js for its core functionality, enriched with the following key libraries:

- **Vue.js** (`vue`): A progressive JavaScript framework for building user interfaces.
- **Fabric.js** (`fabric`): A powerful library for HTML5 canvas manipulation, enabling interactive drawing and object management.
- **Vuedraggable** (`vuedraggable`): A Vue component that allows drag-and-drop functionality, integral for the user interface of retroNode.

Additionally, **core-js** is used to ensure that the application runs smoothly across all modern browsers by providing necessary polyfills.

## Using Tailwind CSS

retroNode utilizes Tailwind CSS for styling. For convenience and ease of use during development, retroNode includes Tailwind CSS via CDN. This setup is sufficient for development and prototyping purposes. However, if you plan to use retroNode in a production environment, or if you need to customize Tailwind's configuration, you may consider installing Tailwind CSS in your project.

## To-do

As of publication, the Zoom is a bit wonky, since I am having troubles calculating the vanishing point. It is a work in progress.   

## Getting Started

1. **Prerequisites**:
   - Ensure you have [Node.js](https://nodejs.org/) installed.
   - retroNode is built for Vue.js projects. If you haven't already set up a Vue.js environment, you can do so by following the [official Vue.js guide](https://vuejs.org/v2/guide/installation.html).

2. **Clone the Repository**:
   Clone retroNode into your project directory.
   ```sh
   git clone https://github.com/doncabreraphone/retroNode.git
   ```
3. **Install Dependencies**:
   ```sh
   npm install
   ```
4. **Serve the Application**:
   ```sh
   npm run serve
   ```

## Usage Guidelines

- **Initiate the Design Process**: Drag and drop nodes to lay out the game's narrative structure.
- **Scene Configuration**: Utilize the detailed customization options for each node to tailor the narrative experience, incorporating specific imagery, narrative paths, and visibility settings.
- **Leverage the Control Panel**: Access quick operations for adding scenarios, managing application state, and transitioning between development modes.
- **Export and Implement**: Export scenario data and corresponding code snippets for seamless integration with the game logic.
- **Zoom**: Shift + mousewheel to zoom in and out (WIP)
- **Eraser/Drawing**: Ctrl+Z to erase and draw on the canvas

# Get Involved with retroNode

Got ideas or spotted bugs in retroNode? Awesome! It really needs your input! And, I'd love your help making retroNode even better too. Feel free to open up an issue or shoot over a pull request. Let's make this tool rock for all of us in game dev.

## License Stuff

retroNode is rocking the MIT License. That means you're pretty much free to do whatever with it—use it, tweak it, build on it. Just check out the LICENSE file for the legal speak.

## Big Thanks

Shoutout to everyone in the open-source gang and all you cool folks who've contributed to retroNode with your input (Mari, Roy, Fede, Josu, and everyone else). Your feedback have been massive in shaping this tool what it is and I hope you stick around for what's next.





