<template>
  <el-main>
    <!-- タスク入力 -->

    <el-input v-model="input" placeholder="タスク名を入力" clearable>
      <el-button slot="append" size="mini"  icon="el-icon-plus" @click="insertTask">追加</el-button>
    </el-input>

    <!-- タスク一覧 -->
    <p>Todo</p>
    <el-table :data="taskList" :show-header="false" stripe>
      <el-table-column align="center" width="100px">
        <template slot-scope="record">
          <el-button size="mini" type="primary" @click="moveDoneTask(record.$index)" circle plain></el-button>
        </template>

      </el-table-column>
      <el-table-column prop="task" width="auto"></el-table-column>
      <el-table-column align="center" width="100px">
        <template slot-scope="record">
          <el-button size="mini" type="danger" icon="el-icon-delete" @click="deleteTask(record.$index)">削除</el-button>
        </template>

      </el-table-column>

    </el-table>
    <p>Done</p>
    <el-table :data="doneList" :show-header="false" stripe>
      <el-table-column align="center" width="100px">
        <template slot-scope="record">
          <el-button size="mini" type="primary" icon="el-icon-check" @click="moveTodoTask(record.$index)" circle></el-button>
        </template>
      </el-table-column>
      <el-table-column prop="done" width="auto"></el-table-column>
    </el-table>
  </el-main>
</template>

<script>
  export default {
    data () {
      return {
        taskList: [],
        doneList: [],
        input: ''
      }
    },
    methods: {
      // タスク追加
      insertTask () {
        // タスクが未入力の場合、エラーメッセージを表示
        if(this.input == ''){
          this.$message({
            message: 'タスク名が入力されていません。',
            type: 'error',
            duration: 1000
          });
          return false;
        }

        this.taskList.push({task: this.input})
        this.input = ''
      },
      // タスク移動(Todo⇒Done)
      moveDoneTask (index) {
        this.doneList.push({done: this.taskList[index].task})
        this.taskList.splice(index, 1)
      },
      // タスク移動(Done⇒Todo)
      moveTodoTask (index) {
        this.taskList.push({task: this.doneList[index].done})
        this.doneList.splice(index, 1)
      },
      // Todo削除
      deleteTask (index) {
        this.taskList.splice(index, 1)
      }
    }
  }
</script>

<style scoped>
  main.el-main {
    width: 600px;
    margin: 0px auto;
  }
</style>