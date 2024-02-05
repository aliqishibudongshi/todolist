<template>
    <li>
        <label>
        <input
        type="checkbox"
        :checked="todo.done"  @change="handleCheck(todo.id)"
        />
        <span v-show="!todo.isEdit">{{todo.title}}</span>
        <input
        type="text"
        v-show="todo.isEdit"
        :value="todo.title"
        @blur="handleBlur($event, todo)"
        />
        </label>
        <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
        <button class="btn btn-edit" @click="handleUpdate(todo)" v-show="!todo.isEdit">编辑</button>
    </li>
</template>

<script>
import pubsub from "pubsub-js";
export default {
    name: "MyItem",
    props: ["todo"],
    methods: {
        //取消or勾选
        handleCheck(id){
            this.$bus.$emit("checkToDo", id);
        },
        // 删除
        handleDelete(id){
            if(confirm("你确定要删除吗？")){
                // this.$bus.$emit("deleteToDo", id);
                //use pubsub way to achieve above function
                pubsub.publish("deleteToDo", id);
            }
        },
        //更新
        handleUpdate(todo){
            if(Object.prototype.hasOwnProperty.call(todo, "isEdit")){
                todo.isEdit = !todo.isEdit;
            }else{
                this.$set(todo, "isEdit", true);
            }
        },
        //失去焦点回调（真正的更新）
        handleBlur(e, todo){
            todo.isEdit = false;
            if(e.target.value.trim() == "") return alert("输入不能为空");
            //把id和真正更新的title给App
            this.$bus.$emit("updateToDo", e.target.value, todo.id);

        }
    },
}
</script>

<style scoped>
    /*item*/
    li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
    }

    li label {
    float: left;
    cursor: pointer;
    }

    li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
    }

    li button {
    float: right;
    display: none;
    margin-top: 3px;
    }

    li:before {
    content: initial;
    }

    li:last-child {
    border-bottom: none;
    }

    li:hover {
        background-color: #ddd;
    }
    li:hover button{
        display: block;
    }
</style>