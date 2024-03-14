<template>
    <div class="item__content" :class="{ 'line-though': item.status === 'Completed' }">
        {{ item.name }}
    </div>
    <div class="item__option">
        <i class="pi pi-check" @click="$emit('toggleStatus', item.id)"></i>
        <i class="pi pi-trash" @click="$emit('deleteItem', item.id)"></i>
    </div>
</template>


<script>
export default {
    props: {
        item: {
            type: Object,
            required: true
        }
    }
};
</script>