# react_tutorial
-Thẻ html được hỗ trợ ở mọi file mà không cần khai báo. ếu tên thẻ bắt đầu bằng chữ hoa, React hiểu đó là component -> cần khai báo
-Cập nhật DOM khi ko cần thiết sẽ ảnh hưởng tới performance 
-Trang bị reload => State React bị reset
-biến một mảng dữ liệu (Array) thành một danh sách Component UI.
  {posts.map((post) => (
    <Post key={post.body} author={post.author} body={post.body} />
  ))}

###
- useState -> Lưu dữ liệu và render lại khi dữ liệu đổi
- useEffect -> Chạy tác vụ sau khi render => hạy khi component chứa nó render lại và dependency thay đổi.
- useRef -> Lưu dữ liệu mà không render lại

### Crete project use vite

npm create vite@latest my-react-app -- --template react
nvm use 22

###
jsx = html + js
* Truyền value cho JSX

function  Post() {
  const chosenName = Math.random() > 0.5 ? names[0] : names[1];

  return (
    <div>
      <p>{chosenName}</p>
      <p>React.js is awesome!</p>
    </div>
  );
}

export default Post;

### Props
props (viết tắt của properties) là cách truyền dữ liệu từ component cha xuống component con.

Component con
function Welcome(props) {
  return <h1>Xin chào {props.name}</h1>;
}
Component cha
function App() {
  return <Welcome name="Hải" />;
}

### CSS
-Tham khảo cấu trúc folder css tại 05-styling-css-modules   ,   06-exercise-solution-postlist-cmp

### State 
-Trong React, Hook là các hàm đặc biệt cho phép bạn sử dụng các tính năng của React (state, lifecycle, context, ref, ...) bên trong Function Component mà không cần dùng Class Component.
| Hook              | Mục đích chính                                 | Khi nào dùng                            | Ví dụ ngắn                                  |
| ----------------- | ---------------------------------------------- | --------------------------------------- | ------------------------------------------- |
| `useState`        | Lưu và cập nhật state                          | Input, counter, modal, dữ liệu UI       | `const [count, setCount] = useState(0)`     |
| `useEffect`       | Thực hiện side effect sau render               | Gọi API, timer, event listener          | `useEffect(() => {...}, [])`                |
| `useRef`          | Truy cập DOM hoặc lưu giá trị không render lại | Focus input, timer, scroll              | `const inputRef = useRef()`                 |
| `useContext`      | Chia sẻ dữ liệu giữa nhiều component           | User, theme, ngôn ngữ                   | `const user = useContext(UserContext)`      |
| `useMemo`         | Cache kết quả tính toán                        | Filter/sort dữ liệu lớn, tính toán nặng | `useMemo(() => calc(), [data])`             |
| `useCallback`     | Cache function                                 | Truyền callback xuống component con     | `useCallback(() => {}, [])`                 |
| `useReducer`      | Quản lý state phức tạp                         | Form lớn, nhiều trạng thái              | `const [state, dispatch] = useReducer(...)` |
| `useLayoutEffect` | Chạy trước khi browser paint                   | Đo kích thước DOM, animation            | `useLayoutEffect(() => {...}, [])`          |
| Custom Hook       | Tái sử dụng logic                              | Logic dùng ở nhiều component            | `function useCounter() {...}`               |


| Hook         | Gây render lại?        | Lưu dữ liệu? | Truy cập DOM? |
| ------------ | ---------------------- | ------------ | ------------- |
| `useState`   | ✅ Có                   | ✅ Có         | ❌ Không       |
| `useRef`     | ❌ Không                | ✅ Có         | ✅ Có          |
| `useContext` | ✅ Có (khi context đổi) | ✅ Có         | ❌ Không       |
| `useReducer` | ✅ Có                   | ✅ Có         | ❌ Không       |


####
Ý nghĩa của []
useEffect(() => {
  fetchPosts();
}, []);

Mảng thứ 2 gọi là dependency array.

[]

***
Không truyền dependency
useEffect(() => {
  ...
});

Sẽ chạy sau mọi lần render.

Render lần 1 -> chạy
Render lần 2 -> chạy
Render lần 3 -> chạy

### enteredBody
Khi nhiều component cần dùng chung một dữ liệu, hãy đưa state lên component cha gần nhất, rồi truyền xuống các component con bằng props.

Vd: chuyển tới route cha 

function closeHandler() {
  navigate('..');
}

#### useNavigate
- là một Hook của React Router dùng để điều hướng (chuyển trang) bằng code, thay vì người dùng phải click vào <Link> 


### Children
là một prop đặc biệt được React tự động truyền vào component để chứa nội dung nằm giữa thẻ mở và thẻ đóng của component đó.
-> Tham chiếu đến block con nói chung của Component 

Trong file Modal.jsx:

function Modal({ children }) {

dòng này tương đương:

function Modal(props) {
  const children = props.children;
}

### Conditional Content
const [modalIsVisible, setModalIsVisible] = useState(true);

function hideModalHandler() {
  setModalIsVisible(false);
}

let modalContent;

if (modalIsVisible) {
  modalContent = (
    <Modal onClose={hideModalHandler}>
      <NewPost
        onBodyChange={bodyChangeHandler}
        onAuthorChange={authorChangeHandler}
      />
    </Modal>
      );
}


hoặc viết tắt 

return (
  <>
    {modalIsVisible && (
      <Modal onClose={hideModalHandler}>
        <NewPost
          onBodyChange={bodyChangeHandler}
          onAuthorChange={authorChangeHandler}
        />
      </Modal>
    )}
    <ul className={classes.posts}>
      <Post author={enteredAuthor} body={enteredBody} />
      <Post author="Manuel" body="Check out the full course!" />
    </ul>
  </>
);

### fetch data

  useEffect(() => {
    async function fetchPosts() {
      setIsFetching(true);
      const response = await fetch('http://localhost:8080/posts');
      const resData = await response.json();
      setPosts(resData.posts);
      setIsFetching(false);
    }

    fetchPosts();
  }, []);


### add route

1. Tạo danh sách route
const router = createBrowserRouter([
  { path: '/', element: <App /> },
  { path: '/create-post', element: <NewPost /> }
]);

2. Khởi tạo RouterProvider
<RouterProvider router={router} />

RouterProvider sẽ:

Đọc URL hiện tại trên trình duyệt.
So sánh URL với danh sách route.
Render component tương ứng.



####  Example:

import React from 'react'
import ReactDOM from 'react-dom/client'
import { RouterProvider, createBrowserRouter } from 'react-router-dom';

import App from './App'
import NewPost from './components/NewPost';
import './index.css'

const router = createBrowserRouter([
  { path: '/', element: <App /> },
  { path: '/create-post', element: <NewPost /> }
]);

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
)

### Format cấu trúc route

* File main.jsx

import React from 'react';
import ReactDOM from 'react-dom/client';
import { RouterProvider, createBrowserRouter } from 'react-router-dom';

import Posts from './routes/Posts';
import NewPost from './routes/NewPost';
import RootLayout from './routes/RootLayout';
import './index.css';

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    children: [
      {
        path: '/',
        element: <Posts />,
        children: [{ path: '/create-post', element: <NewPost /> }],
      },
    ],
  },
]);

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);

* File Post.jsx

import { Outlet } from 'react-router-dom';

import PostsList from '../components/PostsList';

function Posts() {
  return (
    <>
      <Outlet />
      <main>
        <PostsList />
      </main>
    </>
  );
}

export default Posts;



=>  Outlet giống {children} của React, nhưng thay vì component cha truyền vào, React Router tự động truyền component của route con vào đó.

### Link Navigation
- là cách chuyển trang giữa các route mà không reload lại toàn bộ website.

- Cu the:
React Router cung cấp component:

import { Link } from 'react-router-dom';

<Link to="/about">About</Link>

Khi click:

React Router chặn sự kiện click
URL đổi thành /about
React render component mới
Không reload trang

 * loader()
- cho phép fetch dữ liệu ngay ở route.
- Example:
const router = createBrowserRouter([
  {
    path: '/',
    element: <Posts />,
    loader: postsLoader
  }
]);

async function postsLoader() {
  const response = await fetch(
    'http://localhost:8080/posts'
  );

  return response;
}

- Luồng hoạt động: 
User
  |
  v
Route "/"
  |
  v
loader()
  |
  +--> fetch()
  |
  v
Nhận dữ liệu
  |
  v
Render component

=> Khác với useEffect:

User
  |
  v
Render component
  |
  v
useEffect()
  |
  v
fetch()
  |
  v
Render lại

- Ex:

import { useLoaderData } from 'react-router-dom';

import Post from './Post';
import classes from './PostsList.module.css';

function PostsList() {
  const posts = useLoaderData();

  function addPostHandler(postData) {
    fetch('http://localhost:8080/posts', {
      method: 'POST',
      body: JSON.stringify(postData),
      headers: {
        'Content-Type': 'application/json',
      },
    });
    setPosts((existingPosts) => [postData, ...existingPosts]);
  }

  return (
    <>
      {posts.length > 0 && (
        <ul className={classes.posts}>
          {posts.map((post) => (
            <Post key={post.body} author={post.author} body={post.body} />
          ))}
        </ul>
      )}
      {posts.length === 0 && (
        <div style={{ textAlign: 'center', color: 'white' }}>
          <h2>There are no posts yet.</h2>
          <p>Start adding some!</p>
        </div>
      )}
    </>
  );
}

export default PostsList;

### action()
- là cơ chế xử lý form submission và các thao tác thay đổi dữ liệu (POST, PUT, PATCH, DELETE) ngay trong hệ thống routing.

* Ex:

- Route:

{
  path: '/create-post',
  element: <NewPost />,
  action: action,
}



- Component:

import { Form } from 'react-router-dom';

function NewPost() {
  return (
    <Form method="post">
      <input name="author" />
      <textarea name="body" />
      <button>Submit</button>
    </Form>
  );
}


- Action:

export async function action({ request }) {
  const formData = await request.formData();

  const postData = {
    author: formData.get('author'),
    body: formData.get('body'),
  };

  await fetch('http://localhost:8080/posts', {
    method: 'POST',
    body: JSON.stringify(postData),
    headers: {
      'Content-Type': 'application/json',
    },
  });

  return redirect('/');
}

### So sánh loader và action

| Loader                           | Action                                 |
| -------------------------------- | -------------------------------------- |
| Đọc dữ liệu                      | Ghi dữ liệu                            |
| GET                              | POST/PUT/PATCH/DELETE                  |
| Chạy khi route được tải          | Chạy khi form submit                   |
| `loader({ request })`            | `action({ request })`                  |
| `useLoaderData()` để đọc kết quả | Thường dùng `redirect()` sau khi xử lý |


### Dynamic Routes
- Dynamic Route + Loader là cách dùng phổ biến trong React Router mới.
* Ex:

- Route:
{
  path: '/posts/:postId',
  element: <PostDetails />,
  loader: postDetailsLoader
}

- Loader:

export async function postDetailsLoader({ params }) {
  const postId = params.postId;

  const response = await fetch(
    `http://localhost:8080/posts/${postId}`
  );

  return response;
}

- Component:

import { useLoaderData } from 'react-router-dom';

function PostDetails() {
  const post = useLoaderData();

  return (
    <>
      <h1>{post.author}</h1>
      <p>{post.body}</p>
    </>
  );
}

=> Luồng chạy:

/posts/5
    ↓
Route match
    ↓
Loader chạy
    ↓
params.postId = 5
    ↓
fetch /posts/5
    ↓
trả dữ liệu
    ↓
PostDetails render