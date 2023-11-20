# React Event Management

| Nama | Ahmad Mu'min Faisal |
| -- | -- |
| NIM | 1203210101 |
| Kelas | IF-01-03 |
| Link Youtube | https://youtu.be/43WjW07E2iY |
| Frontend Repository | https://github.com/UTS-Semester-5-ITTelkom-Surabaya/react-event-management |
| Backend Repository | https://github.com/UTS-Semester-5-ITTelkom-Surabaya/express-event-management |

## 1 Persiapan Project

### 1.1 Membuat project React

Untuk membuat project React menggunakan Vite, cukup jalankan perintah `yarn create vite`. Kemudian, isikan sesuai identitas project.

### 1.2 Membuat project Express

Untuk membuat project Express, buat folder bernama `backend`, kemudian navigasi ke folder tersebut.

```bash
mkdir backend && cd backend
```

Setelah itu, inisiasikan project Node.js dengan perintah:

```bash
npm init
```

Setelah itu, install `express` dengan perintah berikut:

```bash
npm install express
```

Dengan ini, project Express telah dibuat.

## 2 Membuat Aplikasi Express

Aplikasi Express berjalan di port 3000 dan dibuat dengan beberapa endpoint berikut:

| Method | Endpoint | Kegunaan |
| -- | -- | -- |
| GET | http://localhost:3000/events | Mendapatkan data semua event |
| GET | http://localhost:3000/events/images | Mendapatkan data semua gambar |
| GET | http://localhost:3000/events/:id | Mendapatkan detail 1 event berdasarkan `id` |
| POST | http://localhost:3000/events/ | Menambahkan data 1 event |
| PUT | http://localhost:3000/events/:id | Mengedit data 1 event berdasarkan `id` |
| DELETE | http://localhost:3000/events/:id | Menghapus data 1 event berdasarkan `id` |

Untuk source code penuh dapat dilihat di [https://github.com/UTS-Semester-5-ITTelkom-Surabaya/express-event-management](https://github.com/UTS-Semester-5-ITTelkom-Surabaya/express-event-management)

Untuk cuplikan source code dapat dilihat di [Lampiran 2](#lampiran-2-source-code-express)

Pengujian dari aplikasi yang dibuat dapat dilihat di [Lampiran 3](#lampiran-3-testing-pada-postman)


## 3 Membuat Aplikasi React

Aplikasi React ini dibuat menggunakan React Router dan React Query (Tanstack Query). Untuk itu, install package-package berikut:

```bash
yarn add @tanstack/react-query
yarn add react-router-dom
```

Kemudian, bungkus root node dengan kedua provider tersebut.

```jsx
/* other code */

const router = createBrowserRouter([
  {
    path: '/',
    element: <Navigate to="/events" />,
  },
  {
    path: '/events',
    element: <Events />,

    children: [
      {
        path: '/events/new',
        element: <NewEvent />,
      },
    ],
  },
  {
    path: '/events/:id',
    element: <EventDetails />,
    children: [
      {
        path: '/events/:id/edit',
        element: <EditEvent />,
      },
    ],
  },
]);

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <RouterProvider router={router} />
    </QueryClientProvider>
  );
}
/* other code */

```

Setelah itu, komponen-komponen dan halaman dapat dibuat. Berikut adalah tampilan-tampilan dari aplikasi web.

### 3.1 Halaman Utama

![](./assets/main.png)

### 3.2 Daftar Event

![](./assets/list%20event.png)

### 3.3 Menambahkan Event

![](./assets/add%20event.png)

### 3.4 Detail Event

![](./assets/event%20detail.png)

### 3.5 Update Event

![](./assets/update%20event.png)

### 3.6 Delete Event

![](./assets/delete%20event.png)

Untuk source code lengkap dapat diakses di [https://github.com/UTS-Semester-5-ITTelkom-Surabaya/react-event-management](https://github.com/UTS-Semester-5-ITTelkom-Surabaya/react-event-management).

Untuk cuplikan source code dapat diakses di [Lampiran 1](#lampiran-1-source-code-react).

## Lampiran

### Lampiran 1. Source Code React

#### src/App.jsx

```jsx
import React from 'react';
import {
  Navigate,
  RouterProvider,
  createBrowserRouter,
} from 'react-router-dom';
import { QueryClientProvider } from '@tanstack/react-query';

import Events from './components/Events/Events';
import EventDetails from './components/Events/EventDetails';
import NewEvent from './components/Events/NewEvent';
import EditEvent from './components/Events/EditEvent';
import { queryClient } from './util/http';

const router = createBrowserRouter([
  {
    path: '/',
    element: <Navigate to="/events" />,
  },
  {
    path: '/events',
    element: <Events />,

    children: [
      {
        path: '/events/new',
        element: <NewEvent />,
      },
    ],
  },
  {
    path: '/events/:id',
    element: <EventDetails />,
    children: [
      {
        path: '/events/:id/edit',
        element: <EditEvent />,
      },
    ],
  },
]);

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <RouterProvider router={router} />
    </QueryClientProvider>
  );
}

export default App;
```

#### src/index.css

```css
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;700&family=Lato:wght@400;700&display=swap');

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: 'Lato', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: linear-gradient(#222c31, #111d32);
  color: #d9e2f1;
  min-height: 100vh;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#main-header {
  margin: 0;
  padding: 2rem 15%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

#main-header-loading {
  height: 2rem;
  margin-bottom: -2rem;
  text-align: center;
  accent-color: #e30d7c;
}

#header-title {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}

#header-title img {
  width: 4rem;
  height: 4rem;
  border-radius: 50%;
  object-fit: cover;
  filter: drop-shadow(0 2px 8px rgba(0, 0, 0, 0.26));
}

#header-title h1 {
  font-size: 1.5rem;
  color: #fff;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
}

#main-header nav {
  display: flex;
  gap: 1rem;
}

#overview-section {
  margin: -8rem 0 3rem 0;
  padding: 8rem 15%;
  color: #222c31;
  text-align: center;
}

#overview-section h2 {
  font-size: 2.5rem;
  margin: 2rem auto;
  color: #1d161a;
}

#overview-section h2 strong {
  color: #e30d7c;
}

#overview-section p {
  font-size: 1.25rem;
  line-height: 1.5;
  font-family: 'Quicksand', sans-serif;
  text-shadow: 0 2px 2px rgba(0, 0, 0, 0.26);
}

.content-section {
  margin: 3rem 0 6rem 0;
  padding: 0 15%;
}

.content-section h2 {
  font-size: 2rem;
  font-family: 'Quicksand', sans-serif;
  margin: 2rem auto;
  color: #b6cad5;
}

.events-list {
  max-width: 60rem;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(15rem, 1fr));
  gap: 3rem;
}

.event-item {
  height: 100%;
  margin: 2rem 0;
  padding: 0;
  border-radius: 4px;
  background-color: #3c4249;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  gap: 1rem;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 20rem;
}

.event-item img {
  width: 100%;
  object-fit: cover;
}

.event-item-content {
  height: 100%;
  padding: 1rem;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.event-item h2 {
  font-size: 1.15rem;
  font-family: 'Lato', sans-serif;
  margin: 0;
  color: #d7bfcb;
}

.event-item .event-item-date {
  margin: 0.5rem;
  font-size: 0.85rem;
  font-family: 'Quicksand', sans-serif;
}

.event-item .event-item-location {
  margin: 0.5rem;
  font-size: 1rem;
  font-family: 'Quicksand', sans-serif;
}

#search-form input {
  font: inherit;
  padding: 0.5rem 1rem;
  border-radius: 4px 0 0 4px;
  border: none;
  background: #fff;
}

#search-form button {
  font: inherit;
  padding: 0.5rem 1rem;
  border-radius: 0 4px 4px 0;
  border: none;
  background: #b6cad5;
  color: #1d161a;
  font-weight: bold;
  cursor: pointer;
}

#event-form label {
  display: block;
  font-weight: bold;
  font-size: 0.85rem;
  margin-bottom: 0.25rem;
  color: #3c4249;
  text-transform: uppercase;
}

#event-form input,
#event-form textarea {
  font: inherit;
  font-size: 1.1rem;
  padding: 0.25rem;
  border-radius: 4px;
  border: 1px solid #ccc;
  width: 100%;
}

.controls-row {
  display: flex;
  gap: 2rem;
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 2rem;
}

#image-picker p {
  font-weight: bold;
  font-size: 0.85rem;
  margin-bottom: 0.25rem;
  color: #3c4249;
  text-transform: uppercase;
}

#image-picker ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-wrap: wrap;
  overflow-y: auto;
  gap: 0.5rem;
}

#image-picker li {
  width: 4rem;
  height: 3rem;
  border: 2px solid transparent;
  border-radius: 4px;
  overflow: hidden;
}

#image-picker li.selected {
  border-color: #e30d7c;
}

#image-picker img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

#event-details header {
  width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

#event-details header nav {
  display: flex;
  gap: 1rem;
}

#event-details header button,
#event-details header a {
  font: inherit;
  cursor: pointer;
  border: none;
  background-color: transparent;
  color: #b6cad5;
  border-radius: 4px;
  font-weight: bold;
  text-decoration: none;
  padding: 0.25rem;
}

#event-details-content {
  width: 40rem;
  margin: 2rem auto;
  background-color: #343b3f;
  border-radius: 8px;
  overflow: hidden;
}

#event-details-content img {
  width: 100%;
  height: 20rem;
  object-fit: cover;
  margin-bottom: 2rem;
}

#event-details-info {
  padding: 0 3rem 3rem 3rem;
}

#event-details time {
  font-size: 1.5rem;
  margin: 0.5rem 0;
}

#event-details-location {
  font-size: 1.15rem;
  color: #b6cad5;
  font-weight: bold;
  margin: 0;
}

#event-details-description {
  font-size: 1.25rem;
  line-height: 2rem;
  color: #b6cad5;
}

.modal {
  margin: 0;
  padding: 2rem;
  position: fixed;
  top: 10vh;
  left: calc(50% - 15rem);
  width: 30rem;
  max-height: 80vh;
  background: #e2e5eb;
  border: none;
  border-radius: 6px;
  z-index: 100;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  animation: slide-down-fade-in 300ms ease-out forwards;
}

.modal::backdrop {
  height: 100vh;
  width: 100%;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  left: 0;
}

.nav-item {
  text-decoration: none;
  color: #b6cad5;
}

.button {
  font: inherit;
  cursor: pointer;
  padding: 0.5rem 1.5rem;
  border: none;
  background-color: #e30d7c;
  color: #fff;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.26);
  font-weight: bold;
  text-decoration: none;
}

.button:hover {
  background-color: #e30d5b;
  color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
}

.button-text {
  font: inherit;
  cursor: pointer;
  border: none;
  background-color: transparent;
  border: none;
  color: #3f0c26;
  border-radius: 4px;
  font-weight: bold;
  text-decoration: none;
}

.button-text:hover {
  color: #7c184c;
}

.error-block {
  background-color: #f0d9e5;
  margin: 1rem 0;
  padding: 1rem;
  border-radius: 4px;
  color: #890b35;
  display: flex;
  gap: 2rem;
  align-items: center;
  text-align: left;
}

.error-block-icon {
  font-size: 2rem;
  width: 3rem;
  height: 3rem;
  color: #fff;
  background-color: #890b35;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.error-block h2 {
  color: inherit;
  font-size: 1.25rem;
  margin: 0;
}

.error-block p {
  margin: 0;
}

.center {
  text-align: center;
}

.lds-ring {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
  margin: 1rem 0;
}
.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid #e30d5b;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #e30d5b transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}

@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes slide-down-fade-in {
  from {
    opacity: 0;
    transform: translateY(-3rem);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

#### src/util/http.js
```jsx
import { QueryClient } from '@tanstack/react-query';

const queryClient = new QueryClient();

async function fetchEvents({ signal, searchTerm }) {
  let url = 'http://localhost:3000/events';

  if (searchTerm) {
    url += `?search=${searchTerm}`;
  }

  const response = await fetch(url, { signal });

  if (!response.ok) {
    const err = new Error('An error occurred while fetching the events');
    err.code = response.status;
    err.info = await response.json();
    throw err;
  }

  const { events } = await response.json();

  return events;
}

async function createNewEvent(eventData) {
  const response = await fetch('http://localhost:3000/events', {
    method: 'POST',
    body: JSON.stringify(eventData),
    headers: {
      'Content-Type': 'application/json',
    },
  });

  if (!response.ok) {
    const error = new Error('An error occurred while creating the event');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  const { event } = await response.json();

  return event;
}

async function fetchSelectableImages({ signal }) {
  const response = await fetch('http://localhost:3000/events/images', {
    signal,
  });

  if (!response.ok) {
    const error = new Error('An error occurred while fetching the images');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  const { images } = await response.json();

  return images;
}

async function fetchEvent({ id, signal }) {
  const response = await fetch(`http://localhost:3000/events/${id}`, {
    signal,
  });

  if (!response.ok) {
    const error = new Error('An error occurred while fetching the event');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  const { event } = await response.json();

  return event;
}

async function deleteEvent({ id }) {
  const response = await fetch(`http://localhost:3000/events/${id}`, {
    method: 'DELETE',
  });

  if (!response.ok) {
    const error = new Error('An error occurred while deleting the event');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  return response.json();
}

async function updateEvent({ id, event }) {
  const response = await fetch(`http://localhost:3000/events/${id}`, {
    method: 'PUT',
    body: JSON.stringify({ event }),
    headers: {
      'Content-Type': 'application/json',
    },
  });

  if (!response.ok) {
    const error = new Error('An error occurred while updating the event');
    error.code = response.status;
    error.info = await response.json();
    throw error;
  }

  return response.json();
}

export {
  queryClient,
  fetchEvents,
  createNewEvent,
  fetchSelectableImages,
  fetchEvent,
  deleteEvent,
  updateEvent,
};
```

#### src/components/Events/EditEvent.jsx
```jsx
/* eslint-disable object-curly-newline */
/* eslint-disable operator-linebreak */
import React from 'react';
import { useQuery, useMutation } from '@tanstack/react-query';
import { Link, useNavigate, useParams } from 'react-router-dom';

import Modal from '../UI/Modal';
import EventForm from './EventForm';
import { fetchEvent, queryClient, updateEvent } from '../../util/http';
import LoadingIndicator from '../UI/LoadingIndicator';
import ErrorBlock from '../UI/ErrorBlock';

export default function EditEvent() {
  const navigate = useNavigate();

  const params = useParams();

  const { data, isPending, isError, error } = useQuery({
    queryKey: ['events', params.id],
    queryFn: ({ signal }) => fetchEvent({ signal, id: params.id }),
  });

  const { mutate } = useMutation({
    mutationFn: updateEvent,
    onMutate: async (newData) => {
      const newEvent = newData.event;

      await queryClient.cancelQueries({ queryKey: ['events', params.id] });
      const previousEvent = queryClient.getQueryData(['events', params.id]);

      queryClient.setQueryData(['events', params.id], newEvent);

      return { previousEvent };
    },
    onError: (_error, _data, context) => {
      queryClient.setQueryData(['events', params.id], context.previousEvent);
    },
    onSettled: () => {
      queryClient.invalidateQueries(['events', params.id]);
    },
  });

  function handleSubmit(formData) {
    mutate({ id: params.id, event: formData });
    navigate('../');
  }

  function handleClose() {
    navigate('../');
  }

  let content;

  if (isPending) {
    content = (
      <div className="center">
        <LoadingIndicator />
      </div>
    );
  }

  if (isError) {
    content = (
      <>
        <ErrorBlock
          title="Failed to load event"
          message={
            error.info?.message ||
            'Failed to load event. Please check your inputs and try again later'
          }
        />
        <div className="form-actions">
          <Link to="../" className="button">
            Okay
          </Link>
        </div>
      </>
    );
  }

  if (data) {
    content = (
      <EventForm
        inputData={data}
        onSubmit={(formData) => handleSubmit(formData)}
      >
        <Link to="../" className="button-text">
          Cancel
        </Link>
        <button type="submit" className="button">
          Update
        </button>
      </EventForm>
    );
  }

  return <Modal onClose={() => handleClose()}>{content}</Modal>;
}
```

#### src/components/Events/EventDetails.jsx
```jsx
/* eslint-disable react/jsx-one-expression-per-line */
/* eslint-disable operator-linebreak */
/* eslint-disable object-curly-newline */
import React, { useState } from 'react';
import { Link, Outlet, useNavigate, useParams } from 'react-router-dom';
import { useQuery, useMutation } from '@tanstack/react-query';

import Header from '../Header';
import { fetchEvent, deleteEvent, queryClient } from '../../util/http';
import ErrorBlock from '../UI/ErrorBlock';
import Modal from '../UI/Modal';

export default function EventDetails() {
  const [isDeleting, setIsDeleting] = useState(false);

  const params = useParams();
  const navigate = useNavigate();

  const { data, isPending, isError, error } = useQuery({
    queryKey: ['events', params.id],
    queryFn: ({ signal }) => fetchEvent({ signal, id: params.id }),
  });

  const {
    mutate,
    isPending: isPendingDeletion,
    isError: isErrorDeleting,
    error: deleteError,
  } = useMutation({
    mutationFn: deleteEvent,
    onSuccess: () => {
      queryClient.invalidateQueries({
        queryKey: ['events'],
        refetchType: 'none',
      });
      navigate('/events');
    },
  });

  function handleStartDelete() {
    setIsDeleting(true);
  }

  function handleStopDelete() {
    setIsDeleting(false);
  }

  function handleDelete() {
    mutate({ id: params.id });
  }

  let content;

  if (isPending) {
    content = (
      <div id="event-details-content" className="center">
        <p>Fetching event data...</p>
      </div>
    );
  }

  if (isError) {
    content = (
      <div id="event-details-content" className="center">
        <ErrorBlock
          title="Failed to load event"
          message={
            error.info?.message ||
            'Failed to fetch event data, please try again later.'
          }
        />
      </div>
    );
  }

  if (data) {
    const formattedDate = new Date(data.date).toLocaleDateString('en-US', {
      day: 'numeric',
      month: 'short',
      year: 'numeric',
    });

    content = (
      <>
        <header>
          <h1>{data.title}</h1>
          <nav>
            <button type="submit" onClick={handleStartDelete}>
              Delete
            </button>
            <Link to="edit">Edit</Link>
          </nav>
        </header>
        <div id="event-details-content">
          <img src={`http://localhost:3000/${data.image}`} alt={data.title} />
          <div id="event-details-info">
            <div>
              <p id="event-details-location">{data.location}</p>
              <time dateTime="Todo-DateT$Todo-Time">
                {formattedDate} @ {data.time}
              </time>
            </div>
            <p id="event-details-description">{data.description}</p>
          </div>
        </div>
      </>
    );
  }

  return (
    <>
      {isDeleting && (
        <Modal onClose={() => handleStopDelete()}>
          <h2>Are you sure?</h2>
          <p>
            Do you really want to delete this event? This action cannot be
            undone.
          </p>
          <div className="form-actions">
            {isPendingDeletion && <p>Deleting, please wait...</p>}
            {!isPendingDeletion && (
              <>
                <button
                  type="submit"
                  onClick={handleStopDelete}
                  className="button-text"
                >
                  Cancel
                </button>
                <button type="submit" onClick={handleDelete} className="button">
                  Delete
                </button>
              </>
            )}
          </div>
          {isErrorDeleting && (
            <ErrorBlock
              title="Failed to delete event"
              message={
                deleteError.info?.message ||
                'Failed to delete event, please try again later.'
              }
            />
          )}
        </Modal>
      )}
      <Outlet />
      <Header>
        <Link to="/events" className="nav-item">
          View all Events
        </Link>
      </Header>
      <article id="event-details">{content}</article>
    </>
  );
}
```

#### src/components/Events/EventForm.jsx
```jsx
/* eslint-disable react/prop-types */
/* eslint-disable jsx-a11y/label-has-associated-control */
import React, { useState } from 'react';
import { useQuery } from '@tanstack/react-query';

import ImagePicker from '../ImagePicker';
import { fetchSelectableImages } from '../../util/http';
import ErrorBlock from '../UI/ErrorBlock';

export default function EventForm({ inputData, onSubmit, children }) {
  const [selectedImage, setSelectedImage] = useState(inputData?.image);

  const { data, isPending, isError } = useQuery({
    queryKey: ['events-images'],
    queryFn: fetchSelectableImages,
  });

  function handleSelectImage(image) {
    setSelectedImage(image);
  }

  function handleSubmit(event) {
    event.preventDefault();

    const formData = new FormData(event.target);
    const data = Object.fromEntries(formData);

    onSubmit({ ...data, image: selectedImage });
  }

  return (
    <form id="event-form" onSubmit={handleSubmit}>
      <p className="control">
        <label htmlFor="title">Title</label>
        <input
          type="text"
          id="title"
          name="title"
          defaultValue={inputData?.title ?? ''}
        />
      </p>

      {isPending && <p>Loading selectable images...</p>}
      {isError && (
        <ErrorBlock
          title="Failed to load selectable images"
          message="Please try again later"
        />
      )}
      {data && (
        <div className="control">
          <ImagePicker
            images={data}
            onSelect={handleSelectImage}
            selectedImage={selectedImage}
          />
        </div>
      )}

      <p className="control">
        <label htmlFor="description">Description</label>
        <textarea
          id="description"
          name="description"
          defaultValue={inputData?.description ?? ''}
        />
      </p>

      <div className="controls-row">
        <p className="control">
          <label htmlFor="date">Date</label>
          <input
            type="date"
            id="date"
            name="date"
            defaultValue={inputData?.date ?? ''}
          />
        </p>

        <p className="control">
          <label htmlFor="time">Time</label>
          <input
            type="time"
            id="time"
            name="time"
            defaultValue={inputData?.time ?? ''}
          />
        </p>
      </div>

      <p className="control">
        <label htmlFor="location">Location</label>
        <input
          type="text"
          id="location"
          name="location"
          defaultValue={inputData?.location ?? ''}
        />
      </p>

      <p className="form-actions">{children}</p>
    </form>
  );
}
```

#### src/components/Events/EventItem.jsx
```jsx
import React from 'react';
import { Link } from 'react-router-dom';

export default function EventItem({ event }) {
  const formattedDate = new Date(event.date).toLocaleDateString('en-US', {
    day: 'numeric',
    month: 'short',
    year: 'numeric',
  });
  return (
    <article className="event-item">
      <img src={`http://localhost:3000/${event.image}`} alt={event.title} />
      <div className="event-item-content">
        <div>
          <h2>{event.title}</h2>
          <p className="event-item-date">{formattedDate}</p>
          <p className="event-item-location">{event.location}</p>
        </div>
        <p>
          <Link to={`/events/${event.id}`} className="button">
            View Details
          </Link>
        </p>
      </div>
    </article>
  );
}
```

#### src/components/Events/Events.jsx
```jsx
import React from 'react';
import { Link, Outlet } from 'react-router-dom';

import Header from '../Header';
import EventsIntroSection from './EventsIntroSection';
import FindEventSection from './FindEventSection';
import NewEventsSection from './NewEventsSection';

export default function Events() {
  return (
    <>
      <Outlet />
      <Header>
        <Link to="/events/new" className="button">
          New Event
        </Link>
      </Header>
      <main>
        <EventsIntroSection />
        <NewEventsSection />
        <FindEventSection />
      </main>
    </>
  );
}
```

#### src/components/Events/EventsIntroSection.jsx
```jsx
import { Link } from 'react-router-dom';

import meetupImg from '../../assets/meetup.jpg';

export default function EventsIntroSection() {
  return (
    <section
      className="content-section"
      id="overview-section"
      style={{ backgroundImage: `url(${meetupImg})` }}
    >
      <h2>
        Connect with amazing people <br />
        or <strong>find a new passion</strong>
      </h2>
      <p>Anyone can organize and join events on React Event!</p>
      <p>
        <Link to="/events/new" className="button">
          Create your first event
        </Link>
      </p>
    </section>
  );
}
```

#### src/components/Events/FindEventSection.jsx
```jsx
import React, { useRef, useState } from 'react';
import { useQuery } from '@tanstack/react-query';
import LoadingIndicator from '../UI/LoadingIndicator';
import ErrorBlock from '../UI/ErrorBlock';
import EventItem from './EventItem';
import { fetchEvents } from '../../util/http';

export default function FindEventSection() {
  const searchElement = useRef();

  const [searchTerm, setSearchTerm] = useState();

  // eslint-disable-next-line object-curly-newline
  const { data, isLoading, isError, error } = useQuery({
    queryKey: ['events', { search: searchTerm }],
    queryFn: ({ signal }) => fetchEvents({ signal, searchTerm }),
    enabled: searchTerm !== undefined,
  });

  function handleSubmit(event) {
    event.preventDefault();
    setSearchTerm(searchElement.current.value);
  }

  let content = <p>Please enter a search term to find events.</p>;

  if (isLoading) {
    content = <LoadingIndicator />;
  }

  if (isError) {
    content = (
      <ErrorBlock
        title="An error occured"
        message={error.info?.message || 'Failed to fetch events'}
      />
    );
  }

  if (data) {
    content = (
      <ul className="events-list">
        {data.map((event) => (
          <li key={event.id}>
            <EventItem event={event} />
          </li>
        ))}
      </ul>
    );
  }

  return (
    <section className="content-section" id="all-events-section">
      <header>
        <h2>Find your next event!</h2>
        <form onSubmit={handleSubmit} id="search-form">
          <input
            type="search"
            placeholder="Search events"
            ref={searchElement}
          />
          <button type="submit">Search</button>
        </form>
      </header>
      {content}
    </section>
  );
}
```

#### src/components/Events/NewEvent.jsx
```jsx
import React from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { useMutation } from '@tanstack/react-query';

import Modal from '../UI/Modal';
import EventForm from './EventForm';
import { createNewEvent, queryClient } from '../../util/http';
import ErrorBlock from '../UI/ErrorBlock';

export default function NewEvent() {
  const navigate = useNavigate();

  // eslint-disable-next-line object-curly-newline
  const { mutate, isPending, isError, error } = useMutation({
    mutationFn: createNewEvent,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['events'] });
      navigate('/events');
    },
  });

  function handleSubmit(formData) {
    mutate({ event: formData });
  }

  return (
    <Modal onClose={() => navigate('../')}>
      <EventForm onSubmit={(formData) => handleSubmit(formData)}>
        {isPending && 'Submitting...'}
        {!isPending && (
          <>
            <Link to="../" className="button-text">
              Cancel
            </Link>
            <button type="submit" className="button">
              Create
            </button>
          </>
        )}
      </EventForm>
      {isError && (
        <ErrorBlock
          title="Failed to create event"
          message={
            // eslint-disable-next-line operator-linebreak
            error.info?.message ||
            'Failed to create event. Please check your inputs and try again later.'
          }
        />
      )}
    </Modal>
  );
}
```

#### src/components/Events/NewEventsSection.jsx
```jsx
import React from 'react';

import { useQuery } from '@tanstack/react-query';

import LoadingIndicator from '../UI/LoadingIndicator';
import ErrorBlock from '../UI/ErrorBlock';
import EventItem from './EventItem';
import { fetchEvents } from '../../util/http';

export default function NewEventsSection() {
  // eslint-disable-next-line object-curly-newline
  const { data, isPending, isError, error } = useQuery({
    queryKey: ['events'],
    queryFn: fetchEvents,
    staleTime: 5000,
  });

  let content;

  if (isPending) {
    content = <LoadingIndicator />;
  }

  if (isError) {
    content = (
      <ErrorBlock
        title="An error occurred"
        message={error.info?.message || 'Failed to fetch events'}
      />
    );
  }

  if (data) {
    content = (
      <ul className="events-list">
        {data.map((event) => (
          <li key={event.id}>
            <EventItem event={event} />
          </li>
        ))}
      </ul>
    );
  }

  return (
    <section className="content-section" id="new-events-section">
      <header>
        <h2>Recently added events</h2>
      </header>
      {content}
    </section>
  );
}
```

#### src/components/UI/ErrorBlock.jsx
```jsx
export default function ErrorBlock({ title, message }) {
  return (
    <div className="error-block">
      <div className="error-block-icon">!</div>
      <div className="error-block-text">
        <h2>{title}</h2>
        <p>{message}</p>
      </div>
    </div>
  );
}
```

#### src/components/UI/LoadingIndicator.jsx
```jsx
export default function LoadingIndicator() {
  return (
    <div className="lds-ring">
      <div></div>
      <div></div>
      <div></div>
      <div></div>
    </div>
  );
}
```

#### src/components/UI/Modal.jsx
```jsx
import { useEffect, useRef } from 'react';
import { createPortal } from 'react-dom';

export default function Modal({ children, onClose }) {
  const dialog = useRef();

  useEffect(() => {
    // Using useEffect to sync the Modal component with the DOM Dialog API
    // This code will open the native <dialog> via it's built-in API whenever the <Modal> component is rendered
    const modal = dialog.current;
    modal.showModal();

    return () => {
      modal.close(); // needed to avoid error being thrown
    };
  }, []);

  return createPortal(
    <dialog className="modal" ref={dialog} onClose={onClose}>
      {children}
    </dialog>,
    document.getElementById('modal')
  );
}
```

#### src/components/Header.jsx
```jsx
import React from 'react';

export default function Header({ children }) {
  return (
    <>
      <div id="main-header-loading"></div>
      <header id="main-header">
        <div id="header-title">
          <h1>React Events</h1>
        </div>
        <nav>{children}</nav>
      </header>
    </>
  );
}
```

#### src/components/ImagePicker.jsx
```jsx
import React from 'react';

export default function ImagePicker({ images, selectedImage, onSelect }) {
  return (
    <div id="image-picker">
      <p>Select an image</p>
      <ul>
        {images.map((image) => (
          <li
            key={image.path}
            onClick={() => onSelect(image.path)}
            className={selectedImage === image.path ? 'selected' : undefined}
          >
            <img
              src={`http://localhost:3000/${image.path}`}
              alt={image.caption}
            />
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Lampiran 2. Source Code Express

#### app.js

```js
import fs from "node:fs/promises";

import bodyParser from "body-parser";
import express from "express";

const app = express();

app.use(bodyParser.json());
app.use(express.static("public"));

app.use((req, res, next) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  res.setHeader(
    "Access-Control-Allow-Methods",
    "GET, POST, PUT, DELETE, OPTIONS"
  );
  res.setHeader(
    "Access-Control-Allow-Headers",
    "X-Requested-With,content-type"
  );
  next();
});

app.get("/events", async (req, res) => {
  const { max, search } = req.query;
  const eventsFileContent = await fs.readFile("./data/events.json");
  let events = JSON.parse(eventsFileContent);

  if (search) {
    events = events.filter((event) => {
      const searchableText = `${event.title} ${event.description} ${event.location}`;
      return searchableText.toLowerCase().includes(search.toLowerCase());
    });
  }

  if (max) {
    events = events.slice(events.length - max, events.length);
  }

  res.json({
    events: events.map((event) => ({
      id: event.id,
      title: event.title,
      image: event.image,
      date: event.date,
      location: event.location,
    })),
  });
});

app.get("/events/images", async (req, res) => {
  const imagesFileContent = await fs.readFile("./data/images.json");
  const images = JSON.parse(imagesFileContent);

  res.json({ images });
});

app.get("/events/:id", async (req, res) => {
  const { id } = req.params;

  const eventsFileContent = await fs.readFile("./data/events.json");
  const events = JSON.parse(eventsFileContent);

  const event = events.find((event) => event.id === id);

  if (!event) {
    return res
      .status(404)
      .json({ message: `For the id ${id}, no event could be found.` });
  }

  setTimeout(() => {
    res.json({ event });
  }, 1000);
});

app.post("/events", async (req, res) => {
  const { event } = req.body;

  if (!event) {
    return res.status(400).json({ message: "Event is required" });
  }

  console.log(event);

  if (
    !event.title?.trim() ||
    !event.description?.trim() ||
    !event.date?.trim() ||
    !event.time?.trim() ||
    !event.image?.trim() ||
    !event.location?.trim()
  ) {
    return res.status(400).json({ message: "Invalid data provided." });
  }

  const eventsFileContent = await fs.readFile("./data/events.json");
  const events = JSON.parse(eventsFileContent);

  const newEvent = {
    id: Math.round(Math.random() * 10000).toString(),
    ...event,
  };

  events.push(newEvent);

  await fs.writeFile("./data/events.json", JSON.stringify(events));

  res.json({ event: newEvent });
});

app.put("/events/:id", async (req, res) => {
  const { id } = req.params;
  const { event } = req.body;

  if (!event) {
    return res.status(400).json({ message: "Event is required" });
  }

  if (
    !event.title?.trim() ||
    !event.description?.trim() ||
    !event.date?.trim() ||
    !event.time?.trim() ||
    !event.image?.trim() ||
    !event.location?.trim()
  ) {
    return res.status(400).json({ message: "Invalid data provided." });
  }

  const eventsFileContent = await fs.readFile("./data/events.json");
  const events = JSON.parse(eventsFileContent);

  const eventIndex = events.findIndex((event) => event.id === id);

  if (eventIndex === -1) {
    return res.status(404).json({ message: "Event not found" });
  }

  events[eventIndex] = {
    id,
    ...event,
  };

  await fs.writeFile("./data/events.json", JSON.stringify(events));

  setTimeout(() => {
    res.json({ event: events[eventIndex] });
  }, 1000);
});

app.delete("/events/:id", async (req, res) => {
  const { id } = req.params;

  const eventsFileContent = await fs.readFile("./data/events.json");
  const events = JSON.parse(eventsFileContent);

  const eventIndex = events.findIndex((event) => event.id === id);

  if (eventIndex === -1) {
    return res.status(404).json({ message: "Event not found" });
  }

  events.splice(eventIndex, 1);

  await fs.writeFile("./data/events.json", JSON.stringify(events));

  setTimeout(() => {
    res.json({ message: "Event deleted" });
  }, 1000);
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

### Lampiran 3. Testing pada Postman

#### GET All Events

![](./assets/GET%20All%20Events.png)

#### GET All Images

![](./assets/GET%20All%20Images.png)

#### GET Single Event by Id

![](./assets/GET%20Single%20Event%20by%20Id.png)

#### POST Single Events

![](./assets/POST%20Single%20Events.png)

#### PUT Single Event by Id

![](./assets/PUT%20Single%20Event%20by%20Id.png)

#### DELETE Single Event by Id

![](./assets/DELETE%20Single%20Event%20by%20Id.png)


