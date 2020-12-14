<template>
  <div class="note">
    <p>{{note.message}}</p>
    <trash-2-icon @click="remove(note.id)" size="1x" class="trash-icon"></trash-2-icon>
  </div>
</template>

<script>
import { Trash2Icon } from "vue-feather-icons";
import { EventBus } from "../main";
export default {
  props: {
    note: {
      type: Object,
      required: true
    }
  },
  components: {
    Trash2Icon
  },
  methods: {
    remove(id) {
      EventBus.$emit("remove", id);
    }
  }
};
</script>

<style lang="scss" scoped>
.note {
  color: #E9E9E9;
  font-family: sans-serif;
  font-weight: 300;
  border: 1px solid #232229;
  padding: 5px 10px;
  border-radius: 5px;
  font-size: 13px;
  word-break: break-all;
  min-height: 100px;
  line-height: 18px;
  font-family: "Chilanka", cursive;

  position: relative;

  &:hover .trash-icon {
    opacity: 1;
  }
}

.trash-icon {
  position: absolute;
  right: -6px;
  top: -6px;
  opacity: 0;
  cursor: pointer;
  transition: all 0.2s;
  user-select: none;
}
</style>