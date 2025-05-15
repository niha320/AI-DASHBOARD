# AI-DASHBOARD
File: src/App.jsx
import React, { useEffect, useState } from 'react';
import AvatarCard from './components/AvatarCard';
import Modal from './components/Modal';

export default function App() {
  const [avatars, setAvatars] = useState([]);
  const [modalOpen, setModalOpen] = useState(false);

  useEffect(() => {
    fetch('https://reqres.in/api/users?per_page=3')
      .then((res) => res.json())
      .then((data) => setAvatars(data.data))
      .catch((err) => console.error('Error fetching avatars:', err));
  }, []);

  const handleAddAvatar = (newAvatar) => {
    setAvatars((prev) => [...prev, { id: Date.now(), ...newAvatar }]);
    setModalOpen(false);
  };

  return (
    <div className="min-h-screen bg-gray-100 p-6">
      <header className="mb-6">
        <h1 className="text-3xl font-bold">Welcome to the AI Dashboard!</h1>
        <p className="text-gray-600">Hello, User!</p>
      </header>

      <section className="grid gap-4 sm:grid-cols-2 md:grid-cols-3">
        {avatars.map((avatar) => (
          <AvatarCard key={avatar.id} avatar={avatar} />
        ))}
      </section>

      <button
        className="fixed bottom-6 right-6 bg-blue-600 text-white px-4 py-2 rounded-full shadow-lg hover:bg-blue-700 transition"
        onClick={() => setModalOpen(true)}
        aria-label="Create New Avatar"
      >
        Create New Avatar
      </button>

      {modalOpen && <Modal onClose={() => setModalOpen(false)} onSubmit={handleAddAvatar} />}
    </div>
  );
}
// File: src/components/AvatarCard.jsx
import React from 'react';

export default function AvatarCard({ avatar }) {
  return (
    <div className="bg-white rounded-xl shadow-md p-4 hover:shadow-lg transition">
      <img
        src={avatar.avatar || 'https://via.placeholder.com/150'}
        alt={${avatar.first_name} ${avatar.last_name}}
        className="w-24 h-24 rounded-full mx-auto object-cover"
      />
      <h2 className="mt-2 text-center text-lg font-semibold">
        {avatar.first_name} {avatar.last_name}
      </h2>
      <p className="text-center text-sm text-gray-500">{avatar.email}</p>
      <div className="text-center mt-2">
        <button className="text-blue-600 hover:underline">Edit</button>
      </div>
    </div>
  );
}
// File: src/components/Modal.jsx
import React, { useState } from 'react';

export default function Modal({ onClose, onSubmit }) {
  const [formData, setFormData] = useState({
    first_name: '',
    last_name: '',
    email: '',
    avatar: '',
  });

  const handleChange = (e) => {
    setFormData((prev) => ({ ...prev, [e.target.name]: e.target.value }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit(formData);
  };
 return (
    <div
      className="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50"
      role="dialog"
      aria-modal="true"
    >
      <div className="bg-white p-6 rounded-xl shadow-lg max-w-md w-full">
        <h2 className="text-xl font-bold mb-4">Create New Avatar</h2>
        <form onSubmit={handleSubmit} className="space-y-4">
          <input
            type="text"
            name="first_name"
            placeholder="First Name"
            value={formData.first_name}
            onChange={handleChange}
            required
            className="w-full p-2 border rounded"
          />
          <input
            type="text"
            name="last_name"
            placeholder="Last Name"
            value={formData.last_name}
            onChange={handleChange}
            required
            className="w-full p-2 border rounded"
          />
          <input
            type="email"
            name="email"
            placeholder="Email"
            value={formData.email}
            onChange={handleChange}
            required
            className="w-full p-2 border rounded"
          />
          <input
            type="url"
            name="avatar"
            placeholder="Avatar Image URL"
            value={formData.avatar}
            onChange={handleChange}
            className="w-full p-2 border rounded"
          />
          <div className="flex justify-end space-x-2">
            <button
              type="button"
              onClick={onClose}
              className="bg-gray-300 px-4 py-2 rounded hover:bg-gray-400"
            >
              Cancel
            </button>
            <button
              type="submit"
              className="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700"
            >
              Save
            </button>
          </div>
        </form>
      </div>
    </div>
  );
}
// File: tailwind.config.js
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
// File: postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
/* File: src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
// File: src/main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
<!-- File: public/index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="AI Dashboard UI with React and Tailwind CSS" />
    <title>AI Dashboard</title>
  </head>
  <body class="antialiased bg-gray-100">
    <div id="root"></div>
  </body>
</html>                                                                                                                                                               
