# expense-tracker
import React, { useState } from 'react';

interface Category {
  id: number;
  name: string;
  budget: number;
  spent: number;
}

interface Expense {
  id: number;
  categoryId: number;
  name: string;
  amount: number;
}

const initialCategories: Category[] = [
  { id: 1, name: 'Housing', budget: 0, spent: 0 },
  { id: 2, name: 'Food', budget: 0, spent: 0 },
  { id: 3, name: 'Transportation', budget: 0, spent: 0 },
];

const initialExpenses: Expense[] = [];

const ExpenseTracker = () => {
  const [categories, setCategories] = useState(initialCategories);
  const [expenses, setExpenses] = useState(initialExpenses);
  const [newCategoryName, setNewCategoryName] = useState('');
  const [newCategoryBudget, setNewCategoryBudget] = useState(0);
  const [selectedCategoryId, setSelectedCategoryId] = useState(0);
  const [newExpenseName, setNewExpenseName] = useState('');
  const [newExpenseAmount, setNewExpenseAmount] = useState(0);

  const handleAddCategory = () => {
    const newCategory: Category = {
      id: categories.length + 1,
      name: newCategoryName,
      budget: newCategoryBudget,
      spent: 0,
    };
    setCategories([...categories, newCategory]);
    setNewCategoryName('');
    setNewCategoryBudget(0);
  };

  const handleAddExpense = () => {
    const newExpense: Expense = {
      id: expenses.length + 1,
      categoryId: selectedCategoryId,
      name: newExpenseName,
      amount: newExpenseAmount,
    };
    setExpenses([...expenses, newExpense]);
    setNewExpenseName('');
    setNewExpenseAmount(0);
    const updatedCategories = categories.map((category) => {
      if (category.id === selectedCategoryId) {
        return { ...category, spent: category.spent + newExpenseAmount };
      }
      return category;
    });
    setCategories(updatedCategories);
  };

  return (
    <div className="max-w-4xl mx-auto p-4">
      <h1 className="text-3xl font-bold mb-4">Expense Tracker</h1>
      <div className="flex flex-col mb-4">
        <h2 className="text-2xl font-bold mb-2">Categories</h2>
        <ul>
          {categories.map((category) => (
            <li key={category.id} className="flex justify-between mb-2">
              <span>{category.name}</span>
              <span>
                Budget: ${category.budget} | Spent: ${category.spent}
              </span>
            </li>
          ))}
        </ul>
        <div className="flex flex-col">
          <input
            type="text"
            value={newCategoryName}
            onChange={(e) => setNewCategoryName(e.target.value)}
            placeholder="New category name"
            className="p-2 mb-2 border border-gray-300 rounded"
          />
          <input
            type="number"
            value={newCategoryBudget}
            onChange={(e) => setNewCategoryBudget(Number(e.target.value))}
            placeholder="New category budget"
            className="p-2 mb-2 border border-gray-300 rounded"
          />
          <button
            onClick={handleAddCategory}
            className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          >
            Add Category
          </button>
        </div>
      </div>
      <div className="flex flex-col mb-4">
        <h2 className="text-2xl font-bold mb-2">Expenses</h2>
        <ul>
          {expenses.map((expense) => (
            <li key={expense.id} className="flex justify-between mb-2">
              <span>{expense.name}</span>
              <span>Amount: ${expense.amount}</span>
            </li>
          ))}
        </ul>
        <div className="flex flex-col">
          <select
            value={selectedCategoryId}
            onChange={(e) => setSelectedCategoryId(Number(e.target.value))}
            className="p-2 mb-2 border border-gray-300 rounded"
          >
            <option value={0}>Select category</option>
            {categories.map((category) => (
              <option key={category.id} value={category.id}>
                {category.name}
              </option>
            ))}
          </select>
          <input
            type="text"
            value={newExpenseName}
            onChange={(e) => setNewExpenseName(e.target.value)}
            placeholder="New expense name"
            className="p-2 mb-2 border border-gray-300 rounded"
          />
          <input
            type="number"
            value={newExpenseAmount}
            onChange={(e) => setNewExpenseAmount(Number(e.target.value))}
            placeholder="New expense amount"
            className="p-2 mb-2 border border-gray-300 rounded"
          />
          <button
            onClick={handleAddExpense}
            className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          >
            Add Expense
          </button>
        </div>
      </div>
    </div>
  );
};

export default ExpenseTracker;
