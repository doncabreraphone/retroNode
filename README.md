# RetroNode

RetroNode is a Vue.js-based tool that makes it easy to design rpg text/graphic adventures through a visual scripting interface. It aims to streamline the game design process, allowing developers and storytellers to construct intricate narrative paths with ease. Each node within RetroNode symbolizes a distinct room or scene, equipped with drag-and-drop capabilities for intuitive spatial arrangement and granular scene configuration.

## Features

- **Visual Scripting Interface**: Allows for intuitive organization of game scenes and rooms through a user-friendly drag-and-drop system.
- **Detailed Scene Customization**: Each node can be customized with images, visibility toggles, and navigational pathways (North, South, East, West) for immersive game experiences.
- **Interactive Scenario Canvas**: Integrates a canvas for rendering interactive scenarios, supporting custom visualization and scenario editing.
- **Control Panel for Quick Access**: Features a floating control panel with buttons for adding scenarios, exporting data, toggling drawing mode, and managing application state.
- **Export Functionality**: Enables exporting scenario data and generated code snippets for easy integration into game logic.

## Dependencies

RetroNode leverages the power of Vue.js for its core functionality, enriched with the following key libraries:

- **Vue.js** (`vue`): A progressive JavaScript framework for building user interfaces.
- **Fabric.js** (`fabric`): A powerful library for HTML5 canvas manipulation, enabling interactive drawing and object management.
- **Vuedraggable** (`vuedraggable`): A Vue component that allows drag-and-drop functionality, integral for the user interface of RetroNode.

Additionally, **core-js** is used to ensure that the application runs smoothly across all modern browsers by providing necessary polyfills.


## Getting Started

1. **Prerequisites**:
   - Ensure you have [Node.js](https://nodejs.org/) installed.
   - RetroNode is built for Vue.js projects. If you haven't already set up a Vue.js environment, you can do so by following the [official Vue.js guide](https://vuejs.org/v2/guide/installation.html).

2. **Clone the Repository**:
   Clone RetroNode into your project directory.
   ```sh
   git clone https://github.com/your-username/RetroNode.git
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

## Contribution and Development

Contributions to RetroNode are highly encouraged. If you have suggestions for improvement or have identified bugs, please feel free to submit an issue or a pull request. Let's collaborate to enhance RetroNode's capabilities for the game development community.

## Licensing

RetroNode is made available under the MIT License, permitting widespread use and modification. For full license details, refer to the LICENSE file.

## Acknowledgements

Special thanks to the open-source community and all contributors who have played a role in the development and refinement of RetroNode, making it a valuable tool for narrative-driven game development.




