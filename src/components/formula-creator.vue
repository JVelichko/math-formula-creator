<template>
  <div class="container">
    <div class="row header">
      <h4>Задать формулу</h4>
      <span class="close"></span>
    </div>
    <div class="row creator">
      <div class="col-2">
        <div class="row flex-column">
          <f-button type="number" value="" @add-label="addLabelHandler"></f-button>
          <f-button type="variable" value="x" @add-label="addLabelHandler"></f-button>
          <f-button type="variable" value="y" @add-label="addLabelHandler"></f-button>
          <f-button type="variable" value="z" @add-label="addLabelHandler"></f-button>
        </div>
      </div>
      <div class="col-9">
        <div class="creator__field">
          <div v-if="labels" class="creator__field-inner">
            <div v-for="label in labels" :key="label.id" class="creator__field-item">
              <f-label :content="label.value" :id="label.id" :type="label.type" :value="label.value" @remove="removeItemsHandler" @on-input="handleInput"></f-label>
            </div>
          </div>
        </div>
      </div>
      <div class="col-1">
        <div class="row flex-column">
          <f-button type="operation" value="+" @add-label="addLabelHandler"></f-button>
          <f-button type="operation" value="-" @add-label="addLabelHandler"></f-button>
          <f-button type="operation" value="*" @add-label="addLabelHandler"></f-button>
          <f-button type="operation" value="/" @add-label="addLabelHandler"></f-button>
          <f-button type="bracketO" value="(" @add-label="addLabelHandler"></f-button>
          <f-button type="bracketC" value=")" @add-label="addLabelHandler" ></f-button>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <h3>{{formula}}</h3>
      </div>
    </div>
  </div>
</template>

<script>
  import FLabel from "./f-label";
  import FButton from "./f-button";

  export default {
    name: "formula-creator",

    components: {
      FLabel,
      FButton
    },

    data() {
      return {
        labels: [],
        count: 0,
        valid: true,
        error: 'Error'
      }
    },

    methods: {
      addLabelHandler: function (type,value) {
        this.labels.push({'type': type, 'value': value, 'id' : 'id'+this.count});
        this.count++
      },
      removeItemsHandler(id) {
        const index = this.labels.findIndex(label => {
          return label.id === id;
        });
        this.labels.splice(index,this.labels.length);
      },
      handleInput(data) {
        const index = this.labels.findIndex(label => {
          return label.id === data.id;
        });
        this.error = data.error;
        this.labels[index].value = data.val;
      },

      isValidformula() {
        this.valid = true;
        let stack = [];

        for(let i = 0; i < this.labels.length; i++) {

          if (this.labels[i].type === 'variable' && i < this.labels.length -1 && this.labels[i].type === this.labels[i+1].type) {
            this.valid = false;
            this.error = "Two variables can't be next to each other";
            break
          }
          if (this.labels[i].type === 'variable' && i < this.labels.length -1 && this.labels[i+1].type === 'bracketO') {
            this.valid = false;
            this.error = "Variable can't be before bracket without operation";
            break
          }
          if (this.labels[i].type === 'variable' && i > 0 && this.labels[i-1].type === 'bracketC') {
            this.valid = false;
            this.error = "Variable can't be after bracket without operation";
            break
          }
          if (this.labels[i].type === 'number' && this.labels[i].value === '') {

            this.valid = false;
            this.error = `Number can't be empty or end with .`;
            break
          }
          //check brackets
          if (this.labels[i].type === 'bracketO') {
            stack.push(this.labels[i].type);
          }
          if (this.labels[i].type === 'bracketC' && stack.pop() !== 'bracketO' ) {
            this.valid = false;
            this.error = "Extra close bracket";
            break;
          }
          if (stack.length) {
            this.error = 'Not all open brackets has close one';
            this.valid = false;
          } else this.valid = true

        }
        return this.valid

      }
    },
    computed: {
      formula: function () {
        if (!this.isValidformula()) {
          return this.error
        } else {
          return this.labels.map(e => e.value).join(" ");
        }

      }
    }
  }
</script>

<style scoped lang="sass">
  @import "src/assets/grid/grid.scss"

  $color-blue: #306BFF
  $color-gray: #c0c0c0
  $color-white: #ffffff

  .header
    background-color: #42b983
    color: $color-white
    height: 56px
    justify-content: space-between
    align-items: center
    padding: 0 16px
    margin-bottom: 16px

    h4
      margin: 0

  .creator
    padding: 0 16px
    height: 50vh

    &__field
      height: 100%
      background-color: $color-gray
      padding: 24px

    &__field-inner
      display: flex
      flex-direction: row
      flex-wrap: wrap
      align-items: flex-start
      margin-right: -8px
      margin-left: -8px
      margin-bottom: -10px

    &__field-item
      padding-left: 8px
      padding-right: 8px
      margin-bottom: 10px

  .close
    background-image: url(data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2216px%22%20height%3D%2216px%22%20viewBox%3D%220%200%2048%2048%22%20fill%3D%22%23ffffff%22%3E%0D%0A%20%20%20%20%3Cpath%20d%3D%22M38%2012.83L35.17%2010%2024%2021.17%2012.83%2010%2010%2012.83%2021.17%2024%2010%2035.17%2012.83%2038%2024%2026.83%2035.17%2038%2038%2035.17%2026.83%2024z%22%2F%3E%0D%0A%20%20%20%20%3Cpath%20d%3D%22M0%200h48v48H0z%22%20fill%3D%22none%22%2F%3E%0D%0A%3C%2Fsvg%3E%0D%0A)
    background-position: center
    background-repeat: no-repeat
    height: 16px
    padding: 12px
    width: 16px
</style>