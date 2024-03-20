<!-- 
    
    This component is the "node" for the visual scripting tool. Each node component represents a room (or scene) in the game. Game designers can drag-and-drop the node to visually organize the spatial layout of rooms within a game. Beyond basic placement, the component allows for detailed customization of each room node, including the ability to delete nodes, upload one image (currently, images can't be replaced and the node has to be deleted and re-created), toggle visibility, and define directional pathways (North, South, East, West) that players can navigate through in the game. 

 -->


<!-- Vue component template for scenario display and interaction -->
<template>
  <div
    ref="scenario"
    :id="`${localScenario.id}`"
    class="scenario w-72 border-2 mb-10 pb-4 pl-4 pr-4 pt-3 rounded-lg bg-gray-50 absolute"
    :style="{
      top: position.top + 'px',
      left: position.left + 'px',
      boxShadow: dragging ? '0 4px 8px rgba(0, 0, 0, 0.2)' : '0 3px 1px #ccc',
      transform: dragging ? `translateY(-10px) scale(${zoomLevel})` : `scale(${zoomLevel})`,
      borderColor: dragging ? '#4ADE80' : '#eee',
      borderWidth: '2px',
      borderStyle: 'solid',
      zIndex: dragging ? '100' : '1',
    }"
    @mouseover="handleMouseOver"
    @mouseout="handleMouseOut"
    @zoom-change="handleZoomChange"
  >
    <h1
      class="text-xl float-left font-mono font-semibold w-4/6"
      @mousedown="dragStart"
      :style="{ cursor: dragging ? 'grabbing' : 'grab' }"
    >
      •{{ localScenario.name }}
    </h1>
    <!-- Scenario buttons (delete, collapse) -->
    <div class="float-right">
      <!-- Button for delete action -->
      <button
        @click="$emit('deleteScenario', scenario.id)"
        class="text-sm text-gray-300 hover:text-black mt-1"
      >
      <!-- Icon for delete action -->
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
          class="w-5 h-5"
        >
          <path
            d="M6.28 5.22a.75.75 0 0 0-1.06 1.06L8.94 10l-3.72 3.72a.75.75 0 1 0 1.06 1.06L10 11.06l3.72 3.72a.75.75 0 1 0 1.06-1.06L11.06 10l3.72-3.72a.75.75 0 0 0-1.06-1.06L10 8.94 6.28 5.22Z"
          />
        </svg>
      </button>
    </div>
    <!-- Button for toggle visibility -->
    <button
      @click="toggleEye"
      class="eye text-sm text-gray-300 hover:text-black mt-1 mr-2 float-right"
    >
    <!-- Icon for toggle visibility -->
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="w-5 h-5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"
        />
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"
        />
      </svg>
    </button>


     <!-- Drop area for image -->
    <div
      v-if="!scenario.scenarioImagePath"
      class="drop-area border-dashed border-2 border-gray-300 rounded-lg p-4 mt-2 text-center clear-right"
      @dragover.prevent
      @dragleave.prevent
      @drop.prevent="handleDrop"
    >
      <p>Drag and drop an image here, or click to select</p>
      <input
        type="file"
        @change="handleFileSelect"
        accept="image/png, image/jpeg, image/gif"
        style="display: none"
        ref="fileInput"
      />
    </div>

    <!-- Display image preview -->
    <div
      v-if="scenario.scenarioImagePath"
      class="mt-2"
      @error="handleImageError"
    >
      <img
        :src="scenario.scenarioImagePath"
        class="max-w-full h-auto rounded-md mb-3"
        @error="handleImageError"
      />
    </div>
    
    <!-- Content section -->
    <div class="content" v-if="!isCollapsed">
      <input
        type="text"
        v-model="localScenario.description"
        placeholder="Description"
        @input="updateScenario('description', localScenario.description)"
        class="w-full my-1 p-2 text-sm text-gray-600 my-1 rounded focus:outline-none focus:border-gray-400"
      />
      <!-- Input field for image URL -->
      <input
        type="text"
        v-model="localScenario.image"
        placeholder="Image URL"
        @input="updateScenario('image', localScenario.image)"
        class="w-full my-1 p-2 text-sm text-gray-600 my-1 rounded focus:outline-none focus:border-gray-400"
      />

      <!-- Directional actions -->
      <div class="mb-3">
        <!-- North - Updated to match West style -->
        <div class="w-100">
          <label
            :for="`north-${localScenario.id}`"
            class="text-gray-500 hover:text-gray-900"
            ><input
              type="checkbox"
              :id="`north-${localScenario.id}`"
              v-model="selectVisible.north"
              @change="updateAction('north')"
              :disabled="scenarios.length < 2"
            />
            North
          </label>

          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="w-5 h-5 inline mr-2 text-gray-500"
          >
            <path
              fill-rule="evenodd"
              d="M2 10a.75.75 0 0 1 .75-.75h12.59l-2.1-1.95a.75.75 0 1 1 1.02-1.1l3.5 3.25a.75.75 0 0 1 0 1.1l-3.5 3.25a.75.75 0 1 1-1.02-1.1l2.1-1.95H2.75A.75.75 0 0 1 2 10Z"
              clip-rule="evenodd"
            />
          </svg>

          <select
            v-if="selectVisible.north"
            v-model="localScenario.actions.north"
            @change="selectCardinalPoint('north', localScenario.actions.north)"
            class="px-2 text-sm"
          >
            <option v-for="s in filteredScenarios" :value="s.name" :key="s.id">
              {{ s.name }}
            </option>
          </select>
        </div>

        <!-- South - Updated to match West style -->
        <div class="w-100">
          <label
            :for="`south-${localScenario.id}`"
            class="text-gray-500 hover:text-gray-900"
            ><input
              type="checkbox"
              :id="`south-${localScenario.id}`"
              v-model="selectVisible.south"
              @change="updateAction('south')"
              :disabled="scenarios.length < 2"
            />
            South
          </label>

          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="w-5 h-5 inline mr-2 text-gray-500"
          >
            <path
              fill-rule="evenodd"
              d="M2 10a.75.75 0 0 1 .75-.75h12.59l-2.1-1.95a.75.75 0 1 1 1.02-1.1l3.5 3.25a.75.75 0 0 1 0 1.1l-3.5 3.25a.75.75 0 1 1-1.02-1.1l2.1-1.95H2.75A.75.75 0 0 1 2 10Z"
              clip-rule="evenodd"
            />
          </svg>

          <select
            v-if="selectVisible.south"
            v-model="localScenario.actions.south"
            class="px-2 text-sm"
          >
            <option v-for="s in filteredScenarios" :value="s.name" :key="s.id">
              {{ s.name }}
            </option>
          </select>
        </div>

        <!-- East - Updated to match West style -->
        <div class="w-100">
          <label
            :for="`east-${localScenario.id}`"
            class="text-gray-500 hover:text-gray-900"
            ><input
              type="checkbox"
              :id="`east-${localScenario.id}`"
              v-model="selectVisible.east"
              @change="updateAction('east')"
              :disabled="scenarios.length < 2"
            />
            East
          </label>

          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="w-5 h-5 inline mr-2 text-gray-500"
          >
            <path
              fill-rule="evenodd"
              d="M2 10a.75.75 0 0 1 .75-.75h12.59l-2.1-1.95a.75.75 0 1 1 1.02-1.1l3.5 3.25a.75.75 0 0 1 0 1.1l-3.5 3.25a.75.75 0 1 1-1.02-1.1l2.1-1.95H2.75A.75.75 0 0 1 2 10Z"
              clip-rule="evenodd"
            />
          </svg>

          <select
            v-if="selectVisible.east"
            v-model="localScenario.actions.east"
            class="px-2 text-sm"
          >
            <option v-for="s in filteredScenarios" :value="s.name" :key="s.id">
              {{ s.name }}
            </option>
          </select>
        </div>

        <div class="w-100">
          <!-- West -->
          <label
            :for="`west-${localScenario.id}`"
            class="text-gray-500 hover:text-gray-900"
            ><input
              type="checkbox"
              :id="`west-${localScenario.id}`"
              v-model="selectVisible.west"
              @change="updateAction('west')"
              :disabled="scenarios.length < 2"
            />
            West
          </label>

          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            class="w-5 h-5 inline mr-2 text-gray-500"
          >
            <path
              fill-rule="evenodd"
              d="M2 10a.75.75 0 0 1 .75-.75h12.59l-2.1-1.95a.75.75 0 1 1 1.02-1.1l3.5 3.25a.75.75 0 0 1 0 1.1l-3.5 3.25a.75.75 0 1 1-1.02-1.1l2.1-1.95H2.75A.75.75 0 0 1 2 10Z"
              clip-rule="evenodd"
            />
          </svg>

          <select
            v-if="selectVisible.west"
            v-model="localScenario.actions.west"
            class="px-2 text-sm"
          >
            <option v-for="s in filteredScenarios" :value="s.name" :key="s.id">
              {{ s.name }}
            </option>
          </select>
        </div>
      </div>

      <hr class="border-gray-400 my-2" />
      <!-- List of Items -->
      <div>
        <label :for="`hasItems-${scenario.id}`">
          <input
            type="checkbox"
            :id="`hasItems-${scenario.id}`"
            v-model="hasItems"
            name="hasItems"
          />
          Has Items
        </label>
      </div>
      <div v-if="hasItems">
        <input
          type="text"
          v-model="localScenario.items"
          placeholder="Enter items separated by comma"
          @input="updateItems"
          class="w-full my-1 p-2 text-sm text-gray-600 my-1 rounded focus:outline-none focus:border-gray-400"
        />
      </div>

      <!-- Display NPC -->
      <div>
        <label :for="`hasNPC-${scenario.id}`">
          <input
            type="checkbox"
            :id="`hasNPC-${scenario.id}`"
            v-model="hasNPC"
            name="hasNPC"
          />
          Has NPC
        </label>
      </div>
      <!-- NPC Name -->
      <div v-if="hasNPC">
        <input
          type="text"
          v-model="localScenario.npc.name"
          placeholder="NPC Name"
          @input="updateNPC('name', localScenario.npc.name)"
          class="w-full my-1 p-2 text-sm text-gray-600 my-1 rounded focus:outline-none focus:border-gray-400"
        />
        <!-- NPC direction -->
        <textarea
          v-model="localScenario.npc.description"
          placeholder="NPC Description"
          @input="updateNPC('description', localScenario.npc.description)"
          class="w-full my-1 p-2 text-sm text-gray-600 my-1 rounded focus:outline-none focus:border-gray-400"
        ></textarea>
      </div>
    </div>
  </div>
</template>



<!-- 
    ## Props and Data Initialization:

    Accepts scenario and scenarios as props to handle the current scenario details and the list of all scenarios, respectively.
    Initializes the component's state (data) with deep-cloned scenario data to prevent direct mutation of props. It sets up defaults for missing NPC (npc) and actions, and calculates initial states for UI elements like visibility of directional actions and NPC presence.
    
    ## Reactivity and Computed Properties:

    Utilizes Vue's computed properties to filter out the current scenario from a list of scenarios and to determine the checked state of directional actions (north, south, east, west).
    
    ## Event Handling and Lifecycle Hooks:

    Employs mouse event listeners (mousemove, mouseup) to implement dragging functionality, enabling users to reposition scenario elements on the interface. Lifecycle hooks are used to add and remove these listeners appropriately.
    Handles image file input through handleFileSelect and handleDrop methods, including reading the file, generating a preview, and optionally resizing the image for performance optimization.
    
    ## Dynamic Styles and Interaction:

    Dynamically adjusts the z-index of scenarios on mouse over to improve usability during editing.
    Implements a method to toggle the collapsed state of scenario details, allowing users to manage screen real estate efficiently.
    
    ## Draggable Interface Implementation:

    Implements methods (dragStart, drag, dragEnd) to support draggable behavior, updating the component's position state based on user interactions. This includes boundary checks and automatic adjustment if the scenario element is dragged beyond a certain threshold.
    
    ## Scenario Data Management:

    Provides methods to update scenario details (actions, NPC data, items) in response to user input, emitting events to notify parent components of changes. This facilitates a modular approach to data management, where the parent component can handle global state updates based on localized changes within each ScenarioComponent.
    
    ## Utility Methods:

    Includes utility methods for common tasks such as checking if a file is an image, extracting and reducing image quality for previews, and parsing positions from CSS for initial load positioning.

 -->
<script>
export default {
  name: "ScenarioComponent",
  props: ["scenario", "scenarios","zoomLevel"],
  data() {
    let initialScenario = JSON.parse(JSON.stringify(this.scenario));
    if (!initialScenario.npc) {
      initialScenario.npc = this.getEmptyNPC();
    }
    if (!initialScenario.actions) {
      initialScenario.actions = {
        north: false,
        south: false,
        east: false,
        west: false,
      };
    }

    const hasNPC =
      !!initialScenario.npc.name || !!initialScenario.npc.description;

    // Update selectVisible based on initial actions
    const hasNorth = !!initialScenario.actions.north;
    const hasSouth = !!initialScenario.actions.south;
    const hasEast = !!initialScenario.actions.east;
    const hasWest = !!initialScenario.actions.west;

    return {
      position: { top: 100, left: 100 },
      dragging: false,
      dragOffset: { x: 0, y: 0 },
      isCollapsed: true,
      imagePreview: null,

      selectVisible: {
        north: hasNorth,
        south: hasSouth,
        east: hasEast,
        west: hasWest,
      },

      localScenario: initialScenario,
      hasItems: !!initialScenario.items && initialScenario.items.length > 0,
      hasNPC: hasNPC,
    };
  },

  watch: {
    zoomLevel(newZoomLevel) {
      console.log(`Zoom level changed to: ${newZoomLevel}`);
      // React to the zoom level change here, e.g., adjust UI elements
      this.adjustToZoomLevel(newZoomLevel);
    },
  },

  mounted() {
    document.addEventListener("mousemove", this.drag);
    document.addEventListener("mouseup", this.dragEnd);
    this.updatePositionFromCSS();
},
beforeUnmount() {
    // Updated from beforeDestroy to beforeUnmount for Vue 3
    document.removeEventListener("mousemove", this.drag);
    document.removeEventListener("mouseup", this.dragEnd);
  },

  computed: {

    filteredScenarios() {
      return this.scenarios.filter((s) => s.id !== this.scenario.id);
    },
    isCheckedNorth() {
      return this.localScenario.actions.north;
    },
    isCheckedSouth() {
      return this.localScenario.actions.south;
    },
    isCheckedEast() {
      return this.localScenario.actions.east;
    },
    isCheckedWest() {
      return this.localScenario.actions.west;
    },
  },
  methods: {

    adjustToZoomLevel(newzoomLevel) {
    // console.log('Received zoom level:', newzoomLevel);


    const scenarioElement = this.$refs.scenario;
    this.$nextTick(() => {
        if (scenarioElement) {
            const scale = `scale(${newzoomLevel})`;
            const offsetX = 0;
            const offsetY = 0;
            const translate = `translate(${offsetX}px, ${offsetY}px)`;

            scenarioElement.style.transform = `${scale} ${translate}`;
            scenarioElement.style.transformOrigin = '0% 0%'; // Top-left of the element itself
            scenarioElement.style.transition = 'transform 0.3s ease';
        }
    });
},


    handleMouseOver() {
      this.$refs.scenario.style.zIndex = "10";
    },
    handleMouseOut() {
      this.$refs.scenario.style.zIndex = "1";
    },
    debugImageData(imageData) {
      console.log(imageData);
    },

    handleImageError(errorEvent) {
      console.error("Error loading image:", errorEvent);
    },
    handleFileSelect(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();

        reader.onload = (e) => {
          this.imagePreview = e.target.result;
        };

        reader.readAsDataURL(file);
      }
    },
    handleDrop(event) {
      const file = event.dataTransfer.files[0];
      this.previewImage(file);
    },

    previewImage(file) {
      if (file && this.isImage(file)) {
        const reader = new FileReader();
        reader.onload = (e) => {
          // Create a new image object
          const img = new Image();
          img.onload = () => {
            // Create a canvas element
            const canvas = document.createElement("canvas");
            const ctx = canvas.getContext("2d");

            // Set canvas size to match the image
            canvas.width = img.width;
            canvas.height = img.height;

            // Draw the image onto the canvas
            ctx.drawImage(img, 0, 0);

            // Convert the canvas content to a Base64 string with reduced quality
            const reducedQualityImage = canvas.toDataURL("image/jpeg", 0.1); // Adjust the quality as needed

            // Use the reduced quality image as the preview
            this.imagePreview = reducedQualityImage;

            // Emit an event with the Base64 data
            this.$emit("imageSelected", {
              scenarioId: this.scenario.id,
              image: reducedQualityImage,
            });
          };
          img.src = e.target.result;
        };
        reader.readAsDataURL(file);
      } else {
        alert("Please select an image file (jpg, gif, png).");
      }
    },
    isImage(file) {
      return file.type.includes("image/");
    },
    openFilePicker() {
      this.$refs.fileInput.click();
    },

    updatePositionFromCSS() {
      const element = this.$el; // Assuming this is the draggable element or you may need to querySelect it specifically.
      const style = window.getComputedStyle(element);
      const left = parseInt(style.left, 10);
      const top = parseInt(style.top, 10);

      // Update the Vue-managed position to match the CSS position.
      // Ensure this only happens if the values are numbers to avoid NaN issues.
      if (
        this.localScenario &&
        this.localScenario.position &&
        !isNaN(this.localScenario.position.x) &&
        !isNaN(this.localScenario.position.y)
      ) {
        // If the CSS-derived positions are valid numbers, update the Vue-managed position.
        if (!isNaN(left) && !isNaN(top)) {
          this.position.left = this.localScenario.position.x;
          this.position.top = this.localScenario.position.y;
        }
      }
    },
    toggleEye() {
      this.isCollapsed = !this.isCollapsed; // Toggle the collapsed state
    },

    dragStart(event) {
      this.dragOffset.x = event.clientX - this.position.left;
      this.dragOffset.y = event.clientY - this.position.top;
      this.dragging = true;
      this.zIndex++;
      event.preventDefault();
    },
    drag(event) {
      if (!this.dragging) return;
      this.position.left = event.clientX - this.dragOffset.x;
      this.position.top = event.clientY - this.dragOffset.y;
    },
    dragEnd() {
      if (!this.dragging) return;
      this.dragging = false;

      // Check if the position exceeds 70% of the viewport width
      const viewportWidth = window.innerWidth;
      const positionExceeded =
        this.position.left + this.$el.offsetWidth > viewportWidth * 0.7;

      if (positionExceeded) {
        this.position.left = 100; // Set to the desired new position
      }

      // Save the new position to localStorage by emiting to App.vue
      this.$emit("update-position", {
        id: this.scenario.id,
        x: this.position.left,
        y: this.position.top,
      });

    },

    adjustInitialPosition() {
      const element = this.$el;
      console.log("adjustInitialPosition Executed");
      this.position.left = element.style.left;
      this.position.top = element.style.top;
    },

    updateAction(direction) {
      console.log(`updateAction called with direction: ${direction}`);

      // Check if action exists (checked previously)
      if (this.localScenario.actions[direction]) {
        // Checkbox unchecked (action exists)
        delete this.localScenario.actions[direction];
        this.$emit(
          "editScenario",
          this.scenario.id,
          "actions",
          this.localScenario.actions
        );
      } 
    },

    updateScenario(field, value) {
      console.log("Updating Scenario");
      this.localScenario[field] = value;
      this.$emit("editScenario", this.scenario.id, field, value);
    },
    updateItems() {
      // Assuming items is a string of items separated by commas
      this.localScenario.items = this.localScenario.items
        .split(",")
        .map((item) => item.trim());
      this.$emit(
        "editScenario",
        this.scenario.id,
        "items",
        this.localScenario.items
      );
    },
    updateNPC(field, value) {
      if (!this.localScenario.npc) {
        this.$set(this.localScenario, "npc", this.getEmptyNPC());
      }
      this.localScenario.npc[field] = value;
      this.$emit(
        "editScenario",
        this.scenario.id,
        "npc",
        this.localScenario.npc
      );
    },
    getEmptyNPC() {
      return {
        name: "",
        description: "",
        dialogue: [],
        currentIndex: 0,
        talking: false,
      };
    },

    getScenarioPositions(scenarioIds) {
      const positions = scenarioIds.map((scenarioId) => {
        // Retrieve and parse the position for each scenario ID from localStorage
        const position = JSON.parse(localStorage.getItem(scenarioId));
        return { scenarioId, position };
      });

      return positions;
    },

   

    selectCardinalPoint(direction, targetScenarioName) {
      console.log(this.localScenario);
      // Only proceed if targetScenarioName is provided
      if (targetScenarioName) {
        // Emit an event with necessary data for the parent component to update and draw connections
        this.$emit("update-and-draw-connection", {
          currentScenario: this.scenario.id,
          direction: direction,
          targetScenarioName: targetScenarioName,
        });

        // Log for debugging
        console.log(
          "Requested connection update between scenarios:",
          this.scenario.id,
          direction,
          targetScenarioName
        );
      }
    },
  },
};
</script>
