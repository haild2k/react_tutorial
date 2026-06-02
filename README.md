# react_tutorial

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

* Props
props (viết tắt của properties) là cách truyền dữ liệu từ component cha xuống component con.

Component con
function Welcome(props) {
  return <h1>Xin chào {props.name}</h1>;
}
Component cha
function App() {
  return <Welcome name="Hải" />;
}