<template>
<div class="container">
  <h2>Todo List</h2>
  <div class="input-group" style="margin-bottom:10px;">
    <input type="text" class="form-control" placeholder="할일을 입력하세요"
            v-model="name"
            v-on:keyup.enter="createTodo(name)">
    <span class="input-group-btn">
      <button class="btn btn-default" type="button"
        @click="createTodo(name)">추가</button>
    </span>
  </div>
  <ul class="list-group">
    <li class="list-group-item" v-for="(todo, index) in todos" :key="todo.id">
      <!--청소하기-->
      {{todo.name}}
      <div class="btn-group pull-right" style="font-size: 12px; line-height: 1;">
        <button type="button" class="btn-link dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          더보기<span class="caret"></span>
        </button>
        <ul class="dropdown-menu">
          <li>
              <a href="#" @click="deleteTodo(todo)">삭제</a>
          </li>
        </ul>
      </div>
    </li>
  </ul>
</div>
</template>

<script>
/*  export default {
    data(){
      return {
        msg:'Example Vue'
      };
    },
    mounted() {
      console.log('Component mounted.')
    }
  }*/
// 임시데이터 추가
  export default {
  name: 'TodoPage',
  data () {
    return {
      // todos: [
      //   {
      //     name:'청소'
      //   },
      //   {
      //     name:'블로그 쓰기'
      //   },
      //   {
      //     name:'밥먹기'
      //   },
      //   {
      //     name:'안녕'
      //   }
      // ]
      name:null,
      thdos:[],
    }
  }
  ,methods:{
    // deleteTodo(i){
		// 	this.todos.splice(i,1);
    // },
    deleteTodo(todo){
      var vm = this;
      this.todos.forEach( function(_todo,i,obj) {// ◇ todos??
        if(_todo.id === todo.id){
			vm.$http.delete('http://todos.garam.xyz/api/todos/'+todo.id)
			.then((result) => {
					obj.splice(i, 1)
			})	
		}
	})
    },
    createTodo(name){
      if(name){
        var vm = this;
        // 2.할일에 추가하도록 
        this.$http.defaults.headers.post['Content-Type'] = 'application/json';
        //this.todos.push({name:name}); // 스크립트로 배열 데이터만든경우
        this.$http.post('http://todos.garam.xyz/api/todos',{
          name:name
        }).then((result) => {
            vm.todos.push(result.data);
        })
        this.name = null
      }
    },
    
    // REST API 로 데이터 가져오기
    getTodos(){
      var vm = this;
      
      this.$http.get('https://todos.garam.xyz/api/todos')
     .then((result) => {
			vm.todos = result.data.data;

        console.log(result);
        // 1.데이터를 todos에 삽입
        vm.todos = result.data.data;
        

        
      })
    }
    
  }
  ,mounted(){
      this.getTodos();
    },
}
</script>