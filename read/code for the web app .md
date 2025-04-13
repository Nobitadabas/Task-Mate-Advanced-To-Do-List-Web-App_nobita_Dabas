Task Mate - Advanced To-Do List Web App

// nobita_Dabas :: project :: 

import React, { useState, useEffect } from 'react';
import TaskList from './components/TaskList';
import AddTaskForm from './components/AddTaskForm';
import SearchBar from './components/SearchBar';
import Filter from './components/Filter';
import './styles.css';

const App = () => {
  const [tasks, setTasks] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [filterCategory, setFilterCategory] = useState('All');

  useEffect(() => {
    const fetchTasks = async () => {
      const response = await fetch('/api/tasks');
      const data = await response.json();
      setTasks(data);
    };
    fetchTasks();
  }, []);

  
// Function to add a new task
  const addTask = async (task) => {
    const response = await fetch('/api/tasks', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(task),
    });
    const newTask = await response.json();
    setTasks([...tasks, newTask]);
  };

  const deleteTask = async (id) => {
    await fetch(`/api/tasks/${id}`, { method: 'DELETE' });
    setTasks(tasks.filter(task => task._id !== id));
  };

  const toggleCompletion = async (id) => {
    const taskToUpdate = tasks.find(task => task._id === id);
    const updatedTask = { ...taskToUpdate, completed: !taskToUpdate.completed };
    await fetch(`/api/tasks/${id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(updatedTask),
    });
    setTasks(tasks.map(task => (task._id === id ? updatedTask : task)));
  };
// Filter tasks based on the search term and selected category
  const filteredTasks = tasks.filter(task => {
    const matchesSearch = task.title.toLowerCase().includes(searchTerm.toLowerCase()) || 
                          task.description.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesCategory = filterCategory === 'All' || task.category === filterCategory;
    return matchesSearch && matchesCategory;
  });

  return (
    <div className="app-container">
      <h1 className="app-title">Task Mate</h1>
      <AddTaskForm addTask={addTask} />
      <SearchBar setSearchTerm={setSearchTerm} />
      <Filter setFilterCategory={setFilterCategory} />
      <TaskList tasks={filteredTasks} deleteTask={deleteTask} toggleCompletion={toggleCompletion} />
      <footer className="motivational-quote">
        <p>"The future depends on what you do today." - Mahatma Gandhi</p>
      </footer>
    </div>
  );
};

export default App;

import React, { useState } from 'react';

const AddTaskForm = ({ addTask }) => {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');
  const [dueDate, setDueDate] = useState('');
  const [category, setCategory] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!title || !description) return;
    addTask({ title, description, dueDate, category, completed: false });
    setTitle('');
    setDescription('');
    setDueDate('');
    setCategory('');
  };

  return (
    23    <form onSubmit={handleSubmit}>
    24      <input
    25        type="text"
    26        placeholder="Title"
    27        value={title}
    28        onChange={(e) => set
