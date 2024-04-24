+++
date = "2024-02-03"
title = "Differences between getServerSideProps and getStaticProps in Next.js"
slug = "differences-between-getserversideprops-and-getstaticprops-in-next.js"
tags = ["React", "Next.js", "JavaScript"]
+++

### Introduction:

`Next.js` is a popular `React framework` that enables developers to create server-side rendered applications with ease. Two essential functions in Next.js are `getServerSideProps` and `getStaticProps`, which help developers pre-render pages and optimize performance. Although they share similarities, they have significant differences in terms of functionality and usage. In this blog, we will explore the differences between getServerSideProps and getStaticProps and provide real-world examples to help you understand their use cases.

### getServerSideProps:

`getServerSideProps` is a Next.js function that runs on the server-side during the request-response cycle. It enables developers to pre-render pages by fetching data from an API or database and passing it as props to the page component. This function is useful when you need to fetch data that changes frequently, as it fetches the data every time a user requests the page.

Here's an example of how to use getServerSideProps:

```javascript
export async function getServerSideProps(context) {
  const response = await fetch('https://api.example.com/posts');
  const data = await response.json();

  return {
    props: {
      data,
    },
  };
}

function Page({ data }) {
  return <div>{data}</div>;
}

export default Page;
```

### getStaticProps:

`getStaticProps` is a Next.js function that runs on the server-side during the build process. It enables developers to pre-render pages by fetching data from an API or database and passing it as props to the page component. Unlike getServerSideProps, getStaticProps fetches the data once during the build process and does not fetch it again during the request-response cycle. This function is useful when you need to fetch data that changes infrequently, as it reduces the number of requests to the API or database.

Here's an example of how to use getStaticProps:

```javascript
export async function getStaticProps() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();

  return {
    props: {
      data,
    },
  };
}

function Page({ data }) {
  return <div>{data}</div>;
}

export default Page;
```

### Differences between getServerSideProps and getStaticProps:

1. **Fetching data**: getServerSideProps fetches data during the request-response cycle, while getStaticProps fetches data during the build process.

2. **Performance**: getStaticProps is faster than getServerSideProps because it fetches data once during the build process, while getServerSideProps fetches data every time a user requests the page.
3. **Data changes**: getServerSideProps is suitable for data that changes frequently, while getStaticProps is suitable for data that changes infrequently.
4. **Static generation**: getStaticProps supports static generation, which means that the page can be generated as a static HTML file during the build process. In contrast, getServerSideProps does not support static generation.
5. **API routes**: getServerSideProps can access Next.js API routes, while getStaticProps cannot.

### Real-world example - getServerSideProps:

Suppose you have a portal that displays product details. The product price changes infrequently and you want to display the latest pricing data for the product. Whenever a user requests product details, it is recommended to utilize `getServerSideProps` to retrieve the necessary data during the pre-rendering phase of the page.

Here's an example of how to use `getServerSideProps` in a real-world scenario:

```javascript
// pages/product/[id].js
import React from 'react';

export async function getServerSideProps({ params }) {
  const { id } = params;
  // Fetch data for the product with the given id
  const res = await fetch(`https://api.example.com/product/${id}`);
  const post = await res.json();

  // Pass data to the page component as props
  return { props: { productDetails } };
}

export default function ProductDetails({ productDetails }) {
  return (
    <div>
      <h1>{productDetails.name}</h1>
      <p>{productDetails.description}</p>
      <p>{productDetails.price}</p>
    </div>
  );
}
```

### Real-world example - getStaticProps:

Suppose you have a blog platform that displays a list of blog posts. The list of blog posts changes infrequently, but you want to display the latest blog posts on the homepage. In this case, you can use `getStaticProps` to fetch the data during the build process and pre-render the page.

Here's an example of how to use `getStaticProps` in a real-world scenario:

```javascript
export async function getStaticProps() {
  const response = await fetch('https://api.example.com/blog-posts');
  const data = await response.json();

  return {
    props: {
      blogPosts: data,
    },
  };
}

function HomePage({ blogPosts }) {
  return (
    <div>
      {blogPosts.map((post) => (
        <div key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
        </div>
      ))}
    </div>
  );
}

export default HomePage;
```

### Conclusion:

In summary, getServerSideProps and getStaticProps are two essential functions in Next.js that help developers pre-render pages and optimize performance. While they share similarities, they have significant differences in terms of functionality and usage. By understanding these differences, developers can choose the appropriate function for their use case and optimize their Next.js applications.
