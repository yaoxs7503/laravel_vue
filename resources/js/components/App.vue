<template>
    <div class="task-component">
 <loading :active.sync="isLoading"></loading>

        <table class="table">
            <thead>
                <tr>
                    <th>任务ID</th>
                    <th>任务标题</th>
                    <th>紧急程度</th>
                    <th>实施情况</th>
                </tr>
            </thead>
            <tbody>
                <task-component v-for="post in posts" :key="post.id" :post="post" @out="remove"></task-component>
            </tbody>
            <tr>
                <td>
                    <input v-model="post.title" type="text" name="" id="task" class="form-control">
                </td>
                <td>
                    <select v-model="post.priority" id="select" class="form-control">
                        <option >等级低</option>
                        <option >中等</option>
                        <option >高</option>
                    </select>
                </td>
                <td>
                    <button @click="store" class="btn btn-primary">Add </button>
                </td>
            </tr>


        </table>
    </div>
</template>

<script>
import TaskComponent from './Task.vue';
// Import component
import Loading from 'vue-loading-overlay';
    // Import stylesheet
import 'vue-loading-overlay/dist/vue-loading.css';
export default {
    data() {
        return {
            isLoading:false,
            posts:[
            ],
            post:{title: '',priority: ''},
            message: 'larvel and vue'
        }
    },
    methods:{
        getPosts(){
        window.axios.get('/api/tasks').then(({data})=>{
                    // console.log(typeof {data});
            data.forEach(task=>{
                this.posts.push(task)
            });
        });
        },
        store(){
            if(this.checkInputs()){
            this.isLoading=true;
            window.axios.post('/api/tasks',this.post).then(savedPost=>{this.posts.push(savedPost.data);
            // console.log(savedPost);
            // console.log(savedPost.data);
            this.post.title='';
            this.isLoading=false;
            });
            }
        },
        checkInputs(){
            if(this.post.title&&this.post.priority){
                return true;
            }
        },
        remove(id){
            this.isLoading=true;
            window.axios.delete(`/api/tasks/${id}`).then(({data})=>{
                let index=this.posts.findIndex(post=>post.id===id);
                this.posts.splice(index,1);
                this.isLoading=false;
            });
            console.log(`我拿到数据了 ${id}`);
        }
    },
    created(){
        this.getPosts();
    },
    // mounted(){
    //     this.getPosts();
    // },
    components:{TaskComponent,Loading}

}
</script>

<style>

</style>

