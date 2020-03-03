<template>
        <tr>
            <td>{{post.id}}</td>
            <td>{{post.title}}</td>
            <td>{{post.priority}}</td>
            <td><button @click="remove" class="btn btn-danger">清除</button></td>
        </tr>
</template>

<script>
export default {
    data() {
        return {

        }
    },
    props:['post'],
    methods:{
        remove(){
            this.$emit('out',this.post.id);

        }
    }
}
</script>

<style>

</style>

