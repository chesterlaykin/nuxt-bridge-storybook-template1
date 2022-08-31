<template>
  <div :class="classes">
    <label
      :for="'checked' + task.id"
      :aria-label="'archiveTask-' + task.id"
      class="checkbox"
    >
      <input
        type="checkbox"
        :checked="isChecked"
        disabled
        :name="'checked' + task.id"
        :id="'archiveTask-' + task.id"
      />
      <span class="checkbox-custom" @click="archiveTask" />
    </label>
    <label :for="'title-' + task.id" :aria-label="task.title" class="title">
      <input
        type="text"
        readonly
        :value="task.title"
        :id="'title-' + task.id"
        name="title"
        placeholder="Input title"
      />
    </label>
    <button
      v-if="!isChecked"
      class="pin-button"
      @click="pinTask"
      :id="'pinTask-' + task.id"
      :aria-label="'pinTask-' + task.id"
    >
      <span class="icon-star" />
    </button>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from "nuxt-property-decorator";
@Component({
  components: {},
})
export default class Task extends Vue {
  @Prop({
    type: Object,
    required: true,
    default: () => ({ id: "", state: "", title: "" }),
    validator: (task: any) =>
      ["id", "state", "title"].every((key: any) => key in task),
  })
  task!: any;

  get classes() {
    return {
      "list-item TASK_INBOX": this.task.state === "TASK_INBOX",
      "list-item TASK_PINNED": this.task.state === "TASK_PINNED",
      "list-item TASK_ARCHIVED": this.task.state === "TASK_ARCHIVED",
    };
  }
  get isChecked() {
    return this.task.state === "TASK_ARCHIVED";
  }

  archiveTask() {
    this.$emit("archive-task", this.task.id);
  }
  pinTask() {
    this.$emit("pin-task", this.task.id);
  }
}
</script>
<style lang="scss" scoped>

.list-item {
  input {
    background: rgb(212, 212, 232);
  }
}

</style>
