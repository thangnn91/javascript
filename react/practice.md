[![Mohammad Faisal](https://res.cloudinary.com/practicaldev/image/fetch/s--nu0zHCBq--/c_fill,f_auto,fl_progressive,h_50,q_auto,w_50/https://dev-to-uploads.s3.amazonaws.com/uploads/user/profile_image/580949/ee64f98c-f816-4153-adb2-dc4f537b22f2.jpeg)](/mohammadfaisal)

[Mohammad Faisal](/mohammadfaisal)
 [betterprogramming.pub](https://betterprogramming.pub/21-best-practices-for-a-clean-react-project-df788a682fb)

 ![](https://dev.to/assets/sparkle-heart-5f9bee3767e18deb1bb725290cb151c25234768a0e9a2bd39370c382d02920cf.svg) 10![](https://dev.to/assets/multi-unicorn-b44d6f8c23cdd00964192bedc38af3e82463978aa611b4365bd33a0f1f4f3e97.svg) 1   ![](https://dev.to/assets/exploding-head-daceb38d627e6ae9b730f36a1e390fca556a4289d5a41abb2c35068ad3e2c4b5.svg)  ![](https://dev.to/assets/raised-hands-74b2099fd66a39f2d7eed9305ee0f4553df0eb7b4f11b01b6b1b499973048fe5.svg)![](https://dev.to/assets/fire-f60e7a582391810302117f987b22a8ef04a2fe0df7e3258a5f49332df1cec71e.svg) 1

21 Best Practices for a Clean React Project
===========================================

[#react](/t/react) [#javascript](/t/javascript) [#programming](/t/programming) [#aws](/t/aws)

**_To read more articles like this, [visit my blog](https://www.mohammadfaisal.dev/blog)_**

React is very unopinionated about how things should be structured. This is exactly why it’s our responsibility to keep our projects clean and maintainable.

Today, we will discuss some best practices to improve your React application’s health. These rules are widely accepted. As such, having this knowledge is imperative.

Everything will be shown with code, so buckle up!

[](#1-use-jsx-shorthand)1\. Use JSX ShortHand
---------------------------------------------

Try to use JSX shorthand for passing boolean variables. Let’s say you want to control the title visibility of a Navbar component.

### [](#bad)Bad

    return (
      <Navbar showTitle={true} />
    );
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    return(
      <Navbar showTitle />  
    )
    

Enter fullscreen mode Exit fullscreen mode

[](#2-use-ternary-operators)2\. Use Ternary Operators
-----------------------------------------------------

Let’s say you want to show a user’s details based on role.

### [](#bad)Bad

    const { role } = user;
    
    if(role === ADMIN) {
      return <AdminUser />
    }else{
      return <NormalUser />
    } 
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const { role } = user;
    
    return role === ADMIN ? <AdminUser /> : <NormalUser />
    

Enter fullscreen mode Exit fullscreen mode

[](#3-take-advantage-of-object-literals)3\. Take Advantage of Object Literals
-----------------------------------------------------------------------------

Object literals can help make our code more readable. Let’s say you want to show three types of users based on their roles. You can’t use ternary because the number of options exceeds two.

### [](#bad)Bad

    const {role} = user
    
    switch(role){
      case ADMIN:
        return <AdminUser />
      case EMPLOYEE:
        return <EmployeeUser />
      case USER:
        return <NormalUser />
    }
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const {role} = user
    
    const components = {
      ADMIN: AdminUser,
      EMPLOYEE: EmployeeUser,
      USER: NormalUser
    };
    
    const Component = components[role];
    
    return <Componenent />;
    

Enter fullscreen mode Exit fullscreen mode

It looks much better now.

[](#4-use-fragments)4\. Use Fragments
-------------------------------------

Always use `Fragment` over `Div`. It keeps the code clean and is also beneficial for performance because one less node is created in the virtual DOM.

### [](#bad)Bad

    return (
      <div>
         <Component1 />
         <Component2 />
         <Component3 />
      </div>  
    )
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    return (
      <>
         <Component1 />
         <Component2 />
         <Component3 />
      </>  
    )
    

Enter fullscreen mode Exit fullscreen mode

[](#5-dont-define-a-function-inside-render)5\. Don't Define a Function Inside Render
------------------------------------------------------------------------------------

Don’t define a function inside render. Try to keep the logic inside render to an absolute minimum.

### [](#bad)Bad

    return (
        <button onClick={() => dispatch(ACTION_TO_SEND_DATA)}>    // NOTICE HERE
          This is a bad example 
        </button>  
    )
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const submitData = () => dispatch(ACTION_TO_SEND_DATA)
    
    return (
      <button onClick={submitData}>  
        This is a good example 
      </button>  
    )
    

Enter fullscreen mode Exit fullscreen mode

[](#6-use-memo)6\. Use Memo
---------------------------

`React.PureComponent` and `Memo` can significantly improve the performance of your application. They help us to avoid unnecessary rendering.

### [](#bad)Bad

    import React, { useState } from "react";
    
    export const TestMemo = () => {
      const [userName, setUserName] = useState("faisal");
      const [count, setCount] = useState(0);
    
      const increment = () => setCount((count) => count + 1);
    
      return (
        <>
          <ChildrenComponent userName={userName} />
          <button onClick={increment}> Increment </button>
        </>
      );
    };
    
    const ChildrenComponent =({ userName }) => {
      console.log("rendered", userName);
      return <div> {userName} </div>;
    };
    

Enter fullscreen mode Exit fullscreen mode

Although the child component should render only once because the value of count has nothing to do with the `ChildComponent` . But, it renders each time you click on the button.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--31Jwfo52--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://cdn-images-1.medium.com/max/3838/1%2AUC19Qvfj06VAy63lR8mOFg.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--31Jwfo52--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://cdn-images-1.medium.com/max/3838/1%2AUC19Qvfj06VAy63lR8mOFg.png)

### [](#good)Good

Let’s edit the `ChildrenComponent` to this:  

    import React ,{useState} from "react";
    
    const ChildrenComponent = React.memo(({userName}) => {
        console.log('rendered')
        return <div> {userName}</div>
    })
    

Enter fullscreen mode Exit fullscreen mode

Now, no matter how many times you click on the button, it will render only when necessary.

[](#7-put-css-in-javascript)7\. Put CSS in JavaScript
-----------------------------------------------------

Avoid raw JavaScript when writing React applications because organizing CSS is far harder than organizing JS.

### [](#bad)Bad

    // CSS FILE
    
    .body {
      height: 10px;
    }
    
    //JSX
    
    return <div className='body'>
    
    </div>
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const bodyStyle = {
      height: "10px"
    }
    
    return <div style={bodyStyle}>
    
    </div>
    

Enter fullscreen mode Exit fullscreen mode

[](#8-use-object-destructuring)8\. Use Object Destructuring
-----------------------------------------------------------

Use object destructuring to your advantage. Let’s say you need to show a user’s details.

### [](#bad)Bad

    return (
      <>
        <div> {user.name} </div>
        <div> {user.age} </div>
        <div> {user.profession} </div>
      </>  
    )
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const { name, age, profession } = user;
    
    return (
      <>
        <div> {name} </div>
        <div> {age} </div>
        <div> {profession} </div>
      </>  
    )
    

Enter fullscreen mode Exit fullscreen mode

[](#9-string-props-dont-need-curly-braces)9\. String Props Don’t Need Curly Braces
----------------------------------------------------------------------------------

When passing string props to a children component.

### [](#bad)Bad

    return(
      <Navbar title={"My Special App"} />
    )
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    return(
      <Navbar title="My Special App" />  
    )
    

Enter fullscreen mode Exit fullscreen mode

[](#10-remove-js-code-from-jsx)10\. Remove JS Code From JSX
-----------------------------------------------------------

Move any JS code out of JSX if that doesn’t serve any purpose of rendering or UI functionality.

### [](#bad)Bad

    return (
      <ul>
        {posts.map((post) => (
          <li onClick={event => {
            console.log(event.target, 'clicked!'); // <- THIS IS BAD
            }} key={post.id}>{post.title}
          </li>
        ))}
      </ul>
    );
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const onClickHandler = (event) => {
       console.log(event.target, 'clicked!'); 
    }
    
    return (
      <ul>
        {posts.map((post) => (
          <li onClick={onClickHandler} key={post.id}> {post.title} </li>
        ))}
      </ul>
    );
    

Enter fullscreen mode Exit fullscreen mode

[](#11-use-template-literals)11\. Use Template Literals
-------------------------------------------------------

Use template literals to build large strings. Avoid using string concatenation. It’s nice and clean.

### [](#bad)Bad

    const userDetails = user.name + "'s profession is" + user.proffession
    
    return (
      <div> {userDetails} </div>  
    )
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)**Good**

    const userDetails = `${user.name}'s profession is ${user.proffession}`
    
    return (
      <div> {userDetails} </div>  
    )
    

Enter fullscreen mode Exit fullscreen mode

[](#12-import-in-order)12\. Import in Order
-------------------------------------------

Always try to import things in a certain order. It improves code readability.

### [](#bad)Bad

    import React from 'react';
    import ErrorImg from '../../assets/images/error.png';
    import styled from 'styled-components/native';
    import colors from '../../styles/colors';
    import { PropTypes } from 'prop-types';
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

The rule of thumb is to keep the import order like this:

*   Built-in
    
*   External
    
*   Internal
    

So the example above becomes:  

    import React from 'react';
    
    import { PropTypes } from 'prop-types';
    import styled from 'styled-components/native';
    
    import ErrorImg from '../../assets/images/error.png';
    import colors from '../../styles/colors';
    

Enter fullscreen mode Exit fullscreen mode

[](#13-use-implicit-return)13\. Use Implicit return
---------------------------------------------------

Use the JavaScript feature implicit `return` in writing beautiful code. Let’s say your function does a simple calculation and returns the result.

### [](#bad)Bad

    const add = (a, b) => {
      return a + b;
    }
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    const add = (a, b) => a + b;
    

Enter fullscreen mode Exit fullscreen mode

[](#14-component-naming)14\. Component Naming
---------------------------------------------

Always use PascalCase for components and camelCase for instances.

### [](#bad)Bad

    import reservationCard from './ReservationCard';
    
    const ReservationItem = <ReservationCard />;
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    import ReservationCard from './ReservationCard';
    
    const reservationItem = <ReservationCard />;
    

Enter fullscreen mode Exit fullscreen mode

[](#15-reserved-prop-naming)15\. Reserved Prop Naming
-----------------------------------------------------

Don’t use DOM component prop names for passing props between components because others might not expect these names.

### [](#bad)Bad

    <MyComponent style="dark" />
    
    <MyComponent className="dark" />
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    <MyComponent variant="fancy" />
    

Enter fullscreen mode Exit fullscreen mode

[](#16-quotes)16\. Quotes
-------------------------

Use double quotes for JSX attributes and single quotes for all other JS.

### [](#bad)Bad

    
    <Foo bar='bar' />
    
    <Foo style={{ left: "20px" }} />
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    <Foo bar="bar" />
    
    <Foo style={{ left: '20px' }} />
    

Enter fullscreen mode Exit fullscreen mode

[](#17-prop-naming)17\. Prop Naming
-----------------------------------

Always use camelCase for prop names or PascalCase if the prop value is a React component.

### [](#bad)Bad

    <Component
      UserName="hello"
      phone_number={12345678}
    />
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    <MyComponent
      userName="hello"
      phoneNumber={12345678}
      Component={SomeComponent}
    />
    

Enter fullscreen mode Exit fullscreen mode

[](#18-jsx-in-parentheses)18\. JSX in Parentheses
-------------------------------------------------

If your component spans more than one line, always wrap it in parentheses.

### [](#bad)Bad

    return <MyComponent variant="long">
               <MyChild />
             </MyComponent>;
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    
    return (
        <MyComponent variant="long">
          <MyChild />
        </MyComponent>
    );
    

Enter fullscreen mode Exit fullscreen mode

[](#19-selfclosing-tags)19\. Self-Closing Tags
----------------------------------------------

If your component doesn’t have any children, then use self-closing tags. It improves readability.

### [](#bad)Bad

    <SomeComponent variant="stuff"></SomeComponent>
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    <SomeComponent variant="stuff" />
    

Enter fullscreen mode Exit fullscreen mode

[](#20-underscore-in-method-name)20\. Underscore in Method Name
---------------------------------------------------------------

Do not use underscores in any internal React method.

### [](#bad)Bad

    const _onClickHandler = () => {
      // do stuff
    }
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    
    const onClickHandler = () => {
      // do stuff
    }
    

Enter fullscreen mode Exit fullscreen mode

[](#21-alt-prop)21\. Alt Prop
-----------------------------

Always include an alt prop in your `<img >` tags. And don’t use `picture` or `image` in your alt `property` because the screenreaders already announce `img` elements as images. No need to include that.

### [](#bad)Bad

    <img src="hello.jpg" />
    
    <img src="hello.jpg" alt="Picture of me rowing a boat" />
    

Enter fullscreen mode Exit fullscreen mode

### [](#good)Good

    <img src="hello.jpg" alt="Me waving hello" />
    

Enter fullscreen mode Exit fullscreen mode

[](#conclusion)Conclusion
-------------------------

There you go. Congratulations if you’ve made it this far! I hope you learned a thing or two from this article.

I hope you have a wonderful day! :D

**Have something to say?**

[](#resources)Resources
-----------------------

*   Airbnb Guideline: [https://github.com/airbnb/javascript/tree/master/react](https://github.com/airbnb/javascript/tree/master/react)

**Get in touch with me via [LinkedIn](https://www.linkedin.com/in/56faisal/) or my [Personal Website](https://www.mohammadfaisal.dev/).**

Top comments (0)
----------------

Crown

### Sort discussion:

*   [
    
    Selected Sort Option Top
    
    Most upvoted and relevant comments will be first
    
    ](/mohammadfaisal/21-best-practices-for-a-clean-react-project-jdf?comments_sort=top#toggle-comments-sort-dropdown)
*   [
    
    Latest
    
    Most recent comments will be first
    
    ](/mohammadfaisal/21-best-practices-for-a-clean-react-project-jdf?comments_sort=latest#toggle-comments-sort-dropdown)
*   [
    
    Oldest
    
    The oldest comments will be first
    
    ](/mohammadfaisal/21-best-practices-for-a-clean-react-project-jdf?comments_sort=oldest#toggle-comments-sort-dropdown)

Subscribe

    ![pic](https://res.cloudinary.com/practicaldev/image/fetch/s--Ay8S_Y7L--/c_fill,f_auto,fl_progressive,h_90,q_auto,w_90/https://dev-to-uploads.s3.amazonaws.com/uploads/user/profile_image/908846/234c185b-4b0c-4416-b688-ad9a89faba7a.jpeg)

Personal Trusted User

[Create template](/settings/response-templates)

Templates let you quickly answer FAQs or store snippets for re-use.

Submit Preview [Dismiss](/404.html)

[Code of Conduct](/code-of-conduct) • [Report abuse](/report-abuse)

Are you sure you want to hide this comment? It will become hidden in your post, but will still be visible via the comment's [permalink](#).

Hide child comments as well

Confirm

For further actions, you may consider blocking this person and/or [reporting abuse](/report-abuse)
