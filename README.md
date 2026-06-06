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
- useEffect -> Chạy tác vụ sau khi render
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

### enteredBody
Khi nhiều component cần dùng chung một dữ liệu, hãy đưa state lên component cha gần nhất, rồi truyền xuống các component con bằng props.





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

