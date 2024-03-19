<!-- 

    The Vue.js component template consists of a main div container with an id of 'app' and styling classes for centering and full height.

    1. A canvas element (id="scenarioCanvas"): Used for rendering interactive scenarios. It's likely part of a custom drawing or visualization functionality related to scenarios being created or edited.

    2. A control panel (class="fixed mb-5 w-2/3 pr-3 z-10"): This floating panel contains buttons for adding new scenarios, exporting scenarios, toggling drawing mode, loading, and saving application state. Each button is styled consistently and has an attached click event listener that triggers specific methods (e.g., addScenario, exportScenarios, loadAppState, saveAppState).

    3. Informational icons and texts: Inside the control panel, there are SVG icons and text elements indicating the current mode (e.g., "Drawing Mode") and providing interactive options for loading and saving.

    4. The 'exported' div (class="w-1/3 fixed py-3 px-6 right-0 top-0"): This section appears to be a sidebar for displaying exported scenario data and switch case code. It includes text areas where the exported JSON and generated switch case code can be viewed and copied to the clipboard.

    5. ScenarioComponent instances: The template iterates over a 'scenarios' array, rendering a ScenarioComponent for each item. These components are responsible for displaying individual scenarios. They accept props for the scenario data, a reference to the scenarios array, and emit events for deleting, editing, and updating scenarios, among other actions.

-->

<template>
  <div id="app flex justify-center">
    <canvas id="scenarioCanvas"></canvas>
    <div class="max-w-full h-screen p-4">
      <div id="writeBoard" class="fixed mb-5 w-2/3 pr-3 z-10">
        <button
          @click="addScenario"
          class="bg-white border px-4 leading-relaxed border-gray-400 text-gray-500 rounded p-2 transition duration-200 hover:translate-y-[-2px] hover:shadow-lg hover:text-black hover:bg-gray-100"
        >
          Add Scenario
        </button>
        <button
          @click="exportScenarios"
          class="bg-white border px-4 leading-relaxed border-gray-400 text-gray-500 rounded p-2 ml-4 transition duration-200 hover:translate-y-[-2px] hover:shadow-lg hover:text-black hover:bg-gray-100"
        >
          Export Scenarios
        </button>

        <div
          id="drawing"
          class="inline ml-8 text-sm text-gray-500 float-right mr-5 mt-2"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-vector-pen float-left mr-1 mt-1"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M10.646.646a.5.5 0 0 1 .708 0l4 4a.5.5 0 0 1 0 .708l-1.902 1.902-.829 3.313a1.5 1.5 0 0 1-1.024 1.073L1.254 14.746 4.358 4.4A1.5 1.5 0 0 1 5.43 3.377l3.313-.828zm-1.8 2.908-3.173.793a.5.5 0 0 0-.358.342l-2.57 8.565 8.567-2.57a.5.5 0 0 0 .34-.357l.794-3.174-3.6-3.6z"
            />
            <path
              fill-rule="evenodd"
              d="M2.832 13.228 8 9a1 1 0 1 0-1-1l-4.228 5.168-.026.086z"
            />
          </svg>
          Drawing Mode...
        </div>

        <div
          id="load"
          class="inline text-sm text-gray-500 float-right mt-2 hover:text-blue-500 cursor-pointer"
          @click="loadAppState"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="currentColor"
            class="bi bi-folder2 float-left mr-1"
            style="margin-top: -2px"
            viewBox="0 0 16 16"
          >
            <path
              d="M1 3.5A1.5 1.5 0 0 1 2.5 2h2.764c.958 0 1.76.56 2.311 1.184C7.985 3.648 8.48 4 9 4h4.5A1.5 1.5 0 0 1 15 5.5v7a1.5 1.5 0 0 1-1.5 1.5h-11A1.5 1.5 0 0 1 1 12.5zM2.5 3a.5.5 0 0 0-.5.5V6h12v-.5a.5.5 0 0 0-.5-.5H9c-.964 0-1.71-.629-2.174-1.154C6.374 3.334 5.82 3 5.264 3zM14 7H2v5.5a.5.5 0 0 0 .5.5h11a.5.5 0 0 0 .5-.5z"
            />
          </svg>
          Load
        </div>

        <div
          id="load"
          class="inline text-sm text-gray-500 float-right mr-7 mt-2 hover:text-blue-500 cursor-pointer"
          @click="saveAppState"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            fill="currentColor"
            class="bi bi-floppy float-left mr-2"
            viewBox="0 0 16 16"
          >
            <path d="M11 2H9v3h2z" />
            <path
              d="M1.5 0h11.586a1.5 1.5 0 0 1 1.06.44l1.415 1.414A1.5 1.5 0 0 1 16 2.914V14.5a1.5 1.5 0 0 1-1.5 1.5h-13A1.5 1.5 0 0 1 0 14.5v-13A1.5 1.5 0 0 1 1.5 0M1 1.5v13a.5.5 0 0 0 .5.5H2v-4.5A1.5 1.5 0 0 1 3.5 9h9a1.5 1.5 0 0 1 1.5 1.5V15h.5a.5.5 0 0 0 .5-.5V2.914a.5.5 0 0 0-.146-.353l-1.415-1.415A.5.5 0 0 0 13.086 1H13v4.5A1.5 1.5 0 0 1 11.5 7h-7A1.5 1.5 0 0 1 3 5.5V1H1.5a.5.5 0 0 0-.5.5m3 4a.5.5 0 0 0 .5.5h7a.5.5 0 0 0 .5-.5V1H4zM3 15h10v-4.5a.5.5 0 0 0-.5-.5h-9a.5.5 0 0 0-.5.5z"
            />
          </svg>
          Save
        </div>
      </div>

      <div
        id="exported"
        class="w-1/3 fixed py-3 px-6 right-0 top-0 h-screen overflow-auto z-10 bg-white"
      >
        <h1 class="text-lg font-semibold mt-4">
          Scenarios<span
            v-if="showScenarioMessage"
            class="ml-2 text-sm float-right text-green-500"
            >Text copied!</span
          >
        </h1>
        <textarea
          @click="copyTextToClipboard('exportedJson')"
          v-model="exportedJson"
          class="block bg-blue-900 text-white w-full border border-slate-300 rounded-md p-5 shadow-sm focus:outline-none focus:border-sky-200 focus:ring-sky-500 focus:ring-2 sm:text-sm mt-1 h-96"
        ></textarea>

        <h1 class="text-lg font-semibold mt-14">
          Switch Cases<span
            v-if="showSwitchCaseMessage"
            class="ml-2 text-sm float-right text-green-500"
            >Text copied!</span
          >
        </h1>
        <textarea
          @click="copyTextToClipboard('switchCaseCode')"
          v-model="switchCaseCode"
          class="block bg-blue-900 text-white w-full border border-slate-300 rounded-md p-5 shadow-sm focus:outline-none focus:border-sky-200 focus:ring-sky-500 focus:ring-2 sm:text-sm mt-1 h-96"
        ></textarea>
      </div>

      <ScenarioComponent
        v-for="scenario in scenarios"
        ref="scenarioComponents"
        :key="scenario.id"
        :scenario="scenario"
        :scenarios="scenarios"
        @deleteScenario="deleteScenario"
        @editScenario="editScenario"
        @update-position="handleUpdatePosition"
        @updateCanvasDimensions="updateCanvasDimensions"
        @imageSelected="handleImageSelected"
      ></ScenarioComponent>
    </div>
  </div>
</template>


<!-- 
    
- Vue.js component for interactive canvas design and editing, using Fabric.js.
- Enables drawing, erasing, and object manipulation on the canvas.
- Serializes canvas state for persistence, using localStorage.
- Integrates custom Vue components and user input handling (e.g., undo shortcuts).
- Useful for software engineers in game development or interactive storytelling.
- copyTextToClipboard: Copies text from a textarea to the clipboard. Supports two specific textareas (exportedJson and switchCaseCode) and displays a temporary message upon successful copying.
- editScenario: Updates a specific field of a scenario object identified by its ID. Allows in-place editing of scenario details.
- deleteScenario: Removes a scenario from the list of scenarios after confirming the action. Helps manage the scenario list dynamically.
- updateConnection: Updates or creates directional connections between scenarios. Essential for defining navigable paths in an interactive experience.
- changeScenario: Changes the current scenario based on directional input. Includes logic to handle reciprocal connections, enhancing navigability.
- exportScenarios: Converts the list of scenarios into a formatted JavaScript object string (not JSON, despite the method name suggesting otherwise) and a set of switch-case statements based on actions and directions associated with each scenario. This method is crucial for exporting scenario data for use in the application or game logic.

 -->

<script>
/// canvas
import { fabric } from "fabric"; // Import fabric
import { nextTick } from "vue";
/// before canvas
import ScenarioComponent from "./components/ScenarioComponent.vue";

export default {
  name: "App",
  components: {
    ScenarioComponent,
  },
  data() {
    return {
      canvas: null, // Store the Fabric.js canvas object here
      scenarios: [],
      nextId: 0,
      exportedJson: "",
      switchCaseCode: "",
      scenarioConnections: {},
      currentScenario: null,
      temporalSelection: {},
      showScenarioMessage: false,
      showSwitchCaseMessage: false,
      scenarioPositions: {},
    };
  },

  mounted() {
    // Initialize the Fabric.js canvas here
    this.initCanvas();
    window.saveAppState = () => this.saveAppState();
    window.loadAppState = () => this.loadAppState();

    document.addEventListener("keydown", (event) => {
      // Check if CTRL+Z is pressed
      if (event.ctrlKey && event.key === "z") {
        event.preventDefault(); // Prevent the default undo action
        this.toggleDrawingMode(); // Toggle between drawing and erasing
      }
    });
  },

  methods: {
    updateCanvasDimensions() {
      // Assuming initCanvas has been adjusted to be callable as needed,
      // and not just on component mount.
      this.initCanvas();
    },

    handleImageSelected({ scenarioId, image }) {
      console.log("Received scenarioId:", scenarioId); // Debug log
      console.log("Scenarios before find:", this.scenarios); // Debug log
      const scenario = this.scenarios.find((s) => s.id === scenarioId);
      if (scenario) {
        scenario.scenarioImagePath = image;
        console.log("Image path set for scenario:", scenario.scenarioImagePath);
      } else {
        console.log("Scenario not found for ID:", scenarioId); // Debug log
      }
      this.saveAppState(); // Save the updated state with the image data
    },

    loadAppState() {
      const savedStateString = localStorage.getItem("appState");
      if (savedStateString) {
        const savedState = JSON.parse(savedStateString);
        this.scenarios = savedState.scenarios;
        this.scenarioConnections = savedState.scenarioConnections;
        this.switchCaseCode = savedState.switchCaseCode;
        this.exportedJson = savedState.exportedJson;

        // Load scenario image paths and set positions
        savedState.scenarios.forEach((scenario) => {
          const scenarioId = scenario.id;
          const imagePath = scenario.scenarioImagePath; // Get the image path from the saved state

          // Find the corresponding scenario object based on its ID
          const targetScenario = this.scenarios.find(
            (s) => s.id === scenarioId
          );

          if (targetScenario) {
            // Assign the loaded image path to the scenario object
            targetScenario.scenarioImagePath = imagePath;

            // Update positions based on saved state, if positions are saved
            const position = scenario.position;
            if (position) {
              // If the position exists, update the scenario's position
              targetScenario.position = position;
              const positionPayload = {
                id: scenario.id,
                x: scenario.position.x,
                y: scenario.position.y,
              };
              this.handleUpdatePosition(positionPayload);
              this.$emit("update-position", positionPayload);
            }
          } else {
            console.error(`Scenario with ID ${scenarioId} not found.`);
          }
        });

        // After all scenarios have been processed, set the image preview.
        // Check if there are any scenarios and if the first scenario has an image path.
        if (this.scenarios.length > 0 && this.scenarios[0].scenarioImagePath) {
          this.imagePreview = this.scenarios[0].scenarioImagePath;
        }

        this.canvas.loadFromJSON(savedState.canvas, () => {
          this.canvas.renderAll();
        });
      } else {
        console.log("No saved state found in localStorage.");
      }
    },

    handleUpdatePosition(payload) {
      //   console.log("Received position data:", payload);
      this.scenarioPositions[payload.id] = { x: payload.x, y: payload.y };

      const scenarioIndex = this.scenarios.findIndex(
        (scenario) => scenario.id === payload.id
      );
      if (scenarioIndex !== -1) {
        this.scenarios[scenarioIndex].position = { x: payload.x, y: payload.y };
      }

      nextTick().then(() => {
        const scenarioElement = document.getElementById(`${payload.id}`);
        // console.log(scenarioElement);
        if (scenarioElement) {
          //   console.log(`${payload.x}`);
          scenarioElement.style.left = `${payload.x}px`;
          scenarioElement.style.top = `${payload.y}px`;
          //   console.log(`${payload.x}`);
        }
      });
    },

    saveAppState() {
      const canvasJson = this.canvas.toJSON();

      // Update scenario saving to include position data
      const scenariosWithPosition = this.scenarios.map((scenario) => {
        const position = this.scenarioPositions[scenario.id] || { x: 0, y: 0 };
        // Calculate hasItems and hasNPC for each scenario based on the current scenario data
        const hasItems = !!scenario.items && scenario.items.length > 0;
        const hasNPC =
          !!scenario.npc && (scenario.npc.name || scenario.npc.description);

        return {
          ...scenario,
          position,
          hasItems, // Include hasItems in the saved scenario data
          hasNPC, // Include hasNPC in the saved scenario data
        };
      });

      const completeAppState = {
        scenarios: scenariosWithPosition, // Save scenarios with position
        scenarioConnections: this.scenarioConnections,
        switchCaseCode: this.switchCaseCode,
        exportedJson: this.exportedJson,
        canvas: canvasJson,
        scenarioImagePath: this.scenarioImagePath,
      };

      const appStateString = JSON.stringify(completeAppState);
      // console.log("Saved App State:", appStateString); // Log the complete app state string
      localStorage.setItem("appState", appStateString);
    },

    // Assuming you have a method to update scenario positions when they are moved on the canvas
    updateScenarioPosition(scenarioId, newPosition) {
      const scenario = this.scenarios.find((s) => s.id === scenarioId);
      if (scenario) {
        scenario.position = newPosition;
        // Optionally save the updated state to localStorage or another persistence layer
      }
    },

    initCanvas() {
      var canvasEl = document.getElementById("scenarioCanvas");

      // Set canvas size to match the viewport
      canvasEl.width = window.innerWidth;
      canvasEl.height = "4000";

      this.canvas = new fabric.Canvas("scenarioCanvas", {
        width: canvasEl.width,
        height: canvasEl.height,
        isDrawingMode: true,
      });

      // Set drawing parameters for the initial state
      this.canvas.globalCompositeOperation = "source-over"; // Ensure drawing mode
      this.canvas.freeDrawingBrush.color = "blue"; // Initial drawing color
      this.canvas.freeDrawingBrush.width = 3; // Initial brush size

      // Adjust the canvas position, if necessary
      this.canvas.wrapperEl.style.position = "absolute";
      this.canvas.wrapperEl.style.top = "0";
      this.canvas.wrapperEl.style.left = "0";
      this.canvas.wrapperEl.style.zIndex = "";
      // Optionally, add other event listeners or functions here
    },

    toggleDrawingMode() {
      const drawing = document.getElementById("drawing");
      if (this.canvas.globalCompositeOperation === "destination-out") {
        // Switch to drawing mode
        this.canvas.globalCompositeOperation = "source-over"; // Back to drawing
        this.canvas.freeDrawingBrush.color = "blue"; // Your drawing color
        this.canvas.freeDrawingBrush.width = 3; // Your drawing brush size
        drawing.innerHTML = `
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-vector-pen float-left mr-1 mt-1" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M10.646.646a.5.5 0 0 1 .708 0l4 4a.5.5 0 0 1 0 .708l-1.902 1.902-.829 3.313a1.5 1.5 0 0 1-1.024 1.073L1.254 14.746 4.358 4.4A1.5 1.5 0 0 1 5.43 3.377l3.313-.828zm-1.8 2.908-3.173.793a.5.5 0 0 0-.358.342l-2.57 8.565 8.567-2.57a.5.5 0 0 0 .34-.357l.794-3.174-3.6-3.6z"/>
                <path fill-rule="evenodd" d="M2.832 13.228 8 9a1 1 0 1 0-1-1l-4.228 5.168-.026.086z"/>
            </svg>
            Drawing Mode...`;
      } else {
        // Switch to erasing mode
        this.canvas.globalCompositeOperation = "destination-out"; // Enable erasing
        this.canvas.freeDrawingBrush.color = "#eee"; // Match your canvas background
        this.canvas.freeDrawingBrush.width = 30; // Eraser size
        drawing.innerHTML = `
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-eraser-fill float-left mr-1 mt-1" viewBox="0 0 16 16">
                    <path d="M8.086 2.207a2 2 0 0 1 2.828 0l3.879 3.879a2 2 0 0 1 0 2.828l-5.5 5.5A2 2 0 0 1 7.879 15H5.12a2 2 0 0 1-1.414-.586l-2.5-2.5a2 2 0 0 1 0-2.828zm.66 11.34L3.453 8.254 1.914 9.793a1 1 0 0 0 0 1.414l2.5 2.5a1 1 0 0 0 .707.293H7.88a1 1 0 0 0 .707-.293z"/>
                </svg> Eraser Mode...`;
      }
    },

    initializeNextId() {
      if (this.scenarios.length > 0) {
        // Find the maximum ID in the current scenarios
        const maxId = this.scenarios.reduce(
          (max, scenario) => (scenario.id > max ? scenario.id : max),
          this.scenarios[0].id
        );
        // Set nextId to one more than the maximum found ID
        this.nextId = maxId + 1;
      } else {
        // If there are no scenarios, start from 0
        this.nextId = 0;
      }
    },

    addScenario() {
      this.initializeNextId();
      const name = prompt("Enter a name for the new scenario:");
      if (name && !this.scenarios.some((scenario) => scenario.name === name)) {
        this.scenarios.push({
          id: this.nextId++, // Keep the ID for unique identification within Vue component
          name: name,
          image: "",
          description: "",
          items: [],
          actions: { north: "", south: "", east: "", west: "" },
          directionSelected: {
            north: false,
            south: false,
            east: false,
            west: false,
          },
        });
      } else {
        alert("Scenario name is required and must be unique.");
      }
    },

    copyTextToClipboard(textareaId) {
      const textareaValue = this[textareaId];
      navigator.clipboard
        .writeText(textareaValue)
        .then(() => {
          if (textareaId === "exportedJson") {
            this.showScenarioMessage = true;
          } else if (textareaId === "switchCaseCode") {
            this.showSwitchCaseMessage = true;
          }
          setTimeout(() => {
            this.showScenarioMessage = false;
            this.showSwitchCaseMessage = false;
          }, 5000);
        })
        .catch((err) => {
          console.error("Failed to copy text: ", err);
        });
    },

    editScenario(id, field, value) {
      // console.log("Received edited scenario:", id, field, value);
      const scenarioIndex = this.scenarios.findIndex(
        (scenario) => scenario.id === id
      );
      if (scenarioIndex !== -1) {
        const scenarioIndex = this.scenarios.findIndex(
          (scenario) => scenario.id === id
        );
        if (scenarioIndex !== -1) {
          this.scenarios[scenarioIndex][field] = value;
          // Additional logic as needed
        }
      }
    },
    deleteScenario(id) {
      if (confirm("Are you sure you want to delete this scenario?")) {
        this.scenarios = this.scenarios.filter(
          (scenario) => scenario.id !== id
        );
      }
    },

    updateConnection(fromScenario, direction, toScenario) {
      // Ensure the fromScenario entry exists
      if (!this.scenarioConnections[fromScenario]) {
        this.scenarioConnections[fromScenario] = {};
      }
      // Update the specific direction for this scenario
      this.scenarioConnections[fromScenario][direction] = toScenario;
    },

    changeScenario(direction) {
      const nextScenario =
        this.scenarioConnections[this.currentScenario]?.[direction];
      if (nextScenario) {
        this.currentScenario = nextScenario;
      } else {
        // Check for a connection in the opposite direction (reciprocation)
        const reverseDirection = {
          north: "south",
          south: "north",
          east: "west",
          west: "east",
        };
        const connectedScenario =
          this.scenarioConnections[nextScenario]?.[reverseDirection[direction]];
        if (connectedScenario && connectedScenario === this.currentScenario) {
          this.currentScenario = nextScenario;
        }
      }
    },

    exportScenarios() {
      this.exportedJson = ""; // Clear the exported JSON textarea
      this.switchCaseCode = ""; // Clear the switch-case code textarea

      let scenariosObject = "const scenarios = {\n";
      this.scenarios.forEach((scenario, index) => {
        // Convert scenario name to camelCase as object key
        const camelCaseName = scenario.name
          .replace(/(?:^\w|[A-Z]|\b\w)/g, (word, index) =>
            index === 0 ? word.toLowerCase() : word.toUpperCase()
          )
          .replace(/\s+/g, "");

        scenariosObject += `  ${camelCaseName}: {\n`;
        scenariosObject += `    image: "${
          scenario.image ? `ingame/${scenario.image}` : ""
        }",\n`;
        scenariosObject += `    description: "${scenario.description}",\n`;

        // Handle items array
        const items = Array.isArray(scenario.items)
          ? scenario.items
          : (scenario.items || "").split(",").map((item) => item.trim());
        scenariosObject += `    items: [${items
          .map((item) => `"${item}"`)
          .join(", ")}],\n`;

        // Handle NPC, if present
        if (scenario.npc && scenario.npc.name) {
          scenariosObject += `    npc: {\n`;
          scenariosObject += `      name: "${scenario.npc.name}",\n`;
          scenariosObject += `      description: "${scenario.npc.description}"\n`;
          scenariosObject += `    }\n`; // No comma as it's the last item in the object
        } else {
          scenariosObject = scenariosObject.trimEnd();
          scenariosObject = scenariosObject.substring(
            0,
            scenariosObject.length - 1
          ); // Remove last comma
          scenariosObject += "\n"; // Properly end the section
        }

        scenariosObject += `  }${
          index < this.scenarios.length - 1 ? "," : ""
        }\n`; // Add comma except for the last scenario
      });
      scenariosObject += "};\n";

      // Switch case logic remains unchanged
      const actionsByDirection = {};
      this.scenarios.forEach((scenario) => {
        const camelCaseName = scenario.name
          .replace(/(?:^\w|[A-Z]|\b\w)/g, (word, index) =>
            index === 0 ? word.toLowerCase() : word.toUpperCase()
          )
          .replace(/\s+/g, "");
        Object.entries(scenario.actions).forEach(
          ([direction, targetScenarioName]) => {
            if (targetScenarioName) {
              const targetCamelCaseName = String(targetScenarioName)
                .replace(/(?:^\w|[A-Z]|\b\w)/g, (word, index) =>
                  index === 0 ? word.toLowerCase() : word.toUpperCase()
                )
                .replace(/\s+/g, "");
              if (!actionsByDirection[direction]) {
                actionsByDirection[direction] = [];
              }
              actionsByDirection[direction].push({
                from: camelCaseName,
                to: targetCamelCaseName,
              });
            }
          }
        );
      });

      // Generate switch cases from grouped actions
      let switchCases = "switch (action) {\n";
      Object.entries(actionsByDirection).forEach(([direction, actions]) => {
        switchCases += `  case "${direction}":\n`;
        actions.forEach((action, index) => {
          if (index === 0) {
            switchCases += `    if (currentScenario === "${action.from}") newScenario = "${action.to}";\n`;
          } else {
            switchCases += `    else if (currentScenario === "${action.from}") newScenario = "${action.to}";\n`;
          }
        });
        switchCases += "    break;\n";
      });
      switchCases += "  default:\n    \n}\n";

      // Set the formatted output
      this.exportedJson = scenariosObject; // Note: this is now JavaScript code, not JSON
      this.switchCaseCode = switchCases;
    },
  },
};
</script>
