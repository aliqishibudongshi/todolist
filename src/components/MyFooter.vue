<template>
    <div class="todo-footer" v-show="total">
        <label>
            <input type="checkbox" v-model="isAll"/>
        </label>
        <span>
            <span>已完成{{doneTotal}}</span> / 全部{{total}}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
</template>

<script>
export default {
    name: "MyFooter",
    props: ["todos", "checkAllToDo", "clearAllToDo"],
    computed: {
        total(){
            return this.todos.length;        
        },
        doneTotal(){
            return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0);
        },
        //全选或取消全选
        isAll: {
            //当 total 和 doneTotal 一样时才会出现全部勾选
            get(){
                return this.total === this.doneTotal && this.total > 0;
            },
            set(value){
                this.checkAllToDo(value);
            }
        }
    },
    methods: {
        clearAll(){
            //调用App.vue传过来的方法，实现删除已完成。
            this.clearAllToDo();
        }
    }
}
</script>

<style scoped>
    /*footer*/
    .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
    }

    .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
    }

    .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
    }

    .todo-footer button {
    float: right;
    margin-top: 5px;
    }
</style>