Redux 패턴을 사용하여 위의 To-Do List 앱을 구현하기 위해, 상태 관리를 컴포넌트 상태에서 Redux 스토어로 이동하게 됩니다. 여기에서 Redux 설정 및 사용 방법을 보여드리겠습니다.

1. Redux 설치
먼저 Redux와 React-Redux를 설치해야 합니다. 터미널에서 다음 명령을 실행하세요:
bash
npm install @reduxjs/toolkit react-redux
2. Redux 설정
다음은 redux 폴더에 store.js와 tasksSlice.js 파일을 만들어 작업 상태를 관리하는 코드입니다.
tasksSlice.js
javascript
import { createSlice } from '@reduxjs/toolkit';

const tasksSlice = createSlice({
  name: 'tasks',
  initialState: [],
  reducers: {
    addTask: (state, action) => {
      state.push({ id: Date.now().toString(), text: action.payload });
    },
    deleteTask: (state, action) => {
      return state.filter(task => task.id !== action.payload);
    },
  },
});

export const { addTask, deleteTask } = tasksSlice.actions;
export default tasksSlice.reducer;
store.js
javascript
import { configureStore } from '@reduxjs/toolkit';
import tasksReducer from './tasksSlice';

export const store = configureStore({
  reducer: {
    tasks: tasksReducer,
  },
});
3. React Redux 연결
App.js 파일에서 Redux 스토어를 애플리케이션에 연결합니다.
App.js
javascript
import React, { useState } from 'react';
import { Provider, useDispatch, useSelector } from 'react-redux';
import { StyleSheet, Text, TextInput, View, FlatList, Button, TouchableOpacity } from 'react-native';
import { store } from './redux/store';
import { addTask, deleteTask } from './redux/tasksSlice';

const ToDoApp = () => {
  const [task, setTask] = useState('');
  const dispatch = useDispatch();
  const tasks = useSelector(state => state.tasks);

  const handleAddTask = () => {
    if (task.trim()) {
      dispatch(addTask(task));
      setTask('');
    }
  };

  const handleDeleteTask = (id) => {
    dispatch(deleteTask(id));
  };

  return (
    <View style={styles.container}>
      <Text style={styles.header}>To-Do List with Redux</Text>
      <TextInput
        style={styles.input}
        value={task}
        onChangeText={setTask}
        placeholder="Enter a task"
      />
      <Button title="Add Task" onPress={handleAddTask} />
      <FlatList
        data={tasks}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <View style={styles.task}>
            <Text>{item.text}</Text>
            <TouchableOpacity onPress={() => handleDeleteTask(item.id)}>
              <Text style={styles.delete}>Delete</Text>
            </TouchableOpacity>
          </View>
        )}
      />
    </View>
  );
};

export default function App() {
  return (
    <Provider store={store}>
      <ToDoApp />
    </Provider>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  header: {
    fontSize: 24,
    marginBottom: 20,
    textAlign: 'center',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    marginBottom: 10,
    borderRadius: 5,
  },
  task: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    padding: 10,
    marginVertical: 5,
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 5,
  },
  delete: {
    color: 'red',
    fontWeight: 'bold',
  },
});
Redux 구조
tasksSlice.js: 작업 추가/삭제 로직을 포함.
store.js: Redux 스토어를 생성.
App.js: Redux 스토어를 React 컴포넌트에 연결하고 상태를 관리.
위 코드를 사용하여 Redux 패턴으로 상태를 관리하는 To-Do List 앱을 구현할 수 있습니다. 추가적인 기능이나 개선이 필요하다면 알려주세요! 😊