<template>
  <div class="itemspool">
    <label v-html="label" />
    <div class="itemspool__control" v-if="!readOnly">
      <div class="formitem">
        <input
          type="text"
          name="addinput"
          v-model.trim="item"
          placeholder="Enter name"
          :ref="`${name}-pool-input`"
          @keyup.enter="addItem"
        />
      </div>
      <button
        :disabled="!item.length"
        v-text="'Add'"
        class="btn filled sm"
        @click.prevent="addItem"
      />
    </div>
    <span
      class="error"
      v-if="errors.has(name) && !readOnly"
      v-html="`Add at least one item to <b>${name}</b> field`"
    />
    <ul class="itemspool__list">
      <li v-for="(item, i) in itemsList" :key="i">
        <span v-if="showKey">{{ item[showKey] }}</span>
        <span v-else>{{ item }}</span>
        <i
          @click.prevent="deleteItem(i);"
          v-if="!readOnly"
          v-html="'&times;'"
        />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "items-pool",

  inject: ["$validator"],

  data() {
    return {
      item: "",
      itemsList: []
    };
  },

  props: {
    showKey: {
      type: String,
      default: ""
    },
    value: {
      type: Array,
      default: () => []
    },
    label: {
      type: String,
      default: ""
    },
    readOnly: {
      default: false
    },
    name: {
      default: "items-pool"
    }
  },

  mounted() {
    this.itemsList = [...this.value];
    this.checkType();
  },

  watch: {
    value(val) {
      this.itemsList = [...val];
    }
  },

  methods: {
    addItem() {
      const { showKey, item } = this;
      const i = showKey ? { [showKey]: item } : item;
      this.itemsList.push(i);
      this.item = "";
      this.updateParent();
      this.focus();
    },

    deleteItem(i) {
      this.itemsList.splice(i, 1);
      this.updateParent();
      this.focus();
    },

    updateParent() {
      this.$emit("input", this.itemsList);
    },

    focus() {
      this.$refs[`${this.name}-pool-input`].focus();
    }
  }
};
</script>

<style lang="scss">
.itemspool {
  margin-bottom: 30px;
  text-align: left;

  .error {
    color: red;
    font-size: 14px;
  }

  label {
    font-size: 18px;
    font-weight: bold;
  }

  .formitem {
    margin-bottom: 0 !important;
    input {
      font-family: "Avenir", Helvetica, Arial, sans-serif;
      font-size: 16px;
      height: 36px;
      border-radius: 3px;
      background-color: white;
      border: solid 1px #e2e2e7;
      padding: 0 15px;
      width: 100%;
      box-sizing: border-box;
      outline: 0;
      color: #646f79;
      transition: border-color 0.15s;
    }
  }

  &__control {
    display: flex;
    justify-content: flex-start;

    .btn {
      font-family: "Avenir", Helvetica, Arial, sans-serif;
      height: 36px;
      border-radius: 3px;
      font-size: 18px;
      font-weight: 600;
      padding: 0 20px;
      margin-left: 10px;
      text-align: center;
      background-color: white;
      color: #2d333d;
      border: solid 1px #e2e2e7;
      cursor: pointer;

      &[disabled] {
        cursor: not-allowed !important;
      }
    }
  }

  &__list {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    margin-top: 8px;
    padding-left: 0;
    max-width: 500px;

    li {
      display: flex;
      align-items: center;
      line-height: 32px;
      border-radius: 2px;
      background-color: #f3f3f3;
      margin-right: 4px;
      margin-top: 4px;
      padding: 0 8px;
      font-size: 16px;

      span {
        max-width: 150px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }

      i {
        font-style: normal;
        color: #999;
        font-size: 22px;
        margin-left: 8px;

        &:hover {
          color: darken(#999, 10%);
          cursor: pointer;
          transform: scale(1.1);
        }
      }
    }
  }
}
</style>
