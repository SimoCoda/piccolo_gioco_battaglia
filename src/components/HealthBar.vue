<template>
  <section id="monster" class="container">
    <div class="flex items-center justify-center gap-3 mb-2">
      <h2 class="font-bold text-lg">{{ name }} {{life}}%</h2>
      <i class="pi pi-pencil text-lg transition-all hover:text-xl duration-150 cursor-pointer" @click="editName"></i>
    </div>
    <div class="healthbar">
      <div class="healthbar__value" :style="{ width: life + '%' }"></div>
    </div>
  </section>
</template>

<script>
export default {
  name: "HealthBarComponent",
  props: {
    name: { type: String, required: true, default: "Tizio"},
    life: { type: Number, required: true},
  },
  emits: ['newName',],
  setup(props,ctx) {
    const editName = () => {
      ctx.emit('newName');
    }
    return {editName};
  },
};
</script>

