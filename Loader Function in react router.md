<h2>Loading data from API before component load</h2>

<p>While creating route for a page component, let at any page, we need to first get data from api then show the content from data.</p>
<p>We can use loader functionality of react-router-dom</p>
<p><strong>Step1:  </strong>when we create route for any page, we can add a property loader: which value will be a function which will get a params in props, inside this function we can fetch our data using params from api then return the data from function.</p>
<p>
eg:
at post/:postID, we want to show post detail of dyamic postID,<br/><br/>
 loader: async ({ params }) => {<br/>
          const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${params.postId}`);<br/>
          if (!res.ok) {<br/>
            throw new Response("Not Found", { status: 404 });<br/>
          }<br/>
          return res.json();<br/>
        },<br/>
</p>
<br/>

<p><strong>Step2: </strong>Now in our page component, we can access the data using: useLoaderData() from react-router-dom and then using the data, we can display the content.</p>
<p>Eg: <br/>
 
```
eg: import { useLoaderData } from 'react-router-dom';
const data = useLoaderData();
return (
<div className="content-list">
  {
    data.map((item)=>{
  return <Card key={item.id} data={data}/>
});
}
</div>
  )
```

</p>


<h3>This way is much faster than tradition way in which we load the data in component itself.</h3>
<br/>

<h2>For case: when data is taking longer than 2 second to load</h2><br/>
<p>Sometime, some api is slow and it take time to return full data. In that case, we can use <strong>defer</strong> and <strong>await</strong> from react-router-dom</p>
<h4>How to use: </h4>
<p> React-router-dom has a defer function, when we use this defer() and pass an object with promise, it will loader the page component fast. and load the data asynchronously</p>
<p><strong>Step1: </strong> we will import defer from react-router-dom, then in loader: property, we will use defer() and pass an object with promise.</p>
<p>Eg:<br/>
 
 ```
loader: () => {
  return defer({
    all_data: axios.get("https://jsonplaceholder.typicode.com/posts").then(res => res.data)
  });
}
 ```

</p>
<p><strong>Step2: </strong> In the page component, we will import two things: Await from react-router-dom and Suspense from react.</p>
<p>We will use <strong> Suspense with fallback  </strong> for our skeleton element and <strong>Await with resolve</strong> for displaying content.<br/><strong>Await</strong> will resolve the promise and give us data and then we can display the content</p>
<p>Eg: <br/>
 
```

let { all_data } = useLoaderData();
<Suspense fallback={<p className="text-gray-500">Loading cards...</p>}><br/>
        <Await resolve={all_data}>
          {(data) => (
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
              {data.map((card) => (
                <div key={card.id} className="border p-4 rounded shadow">
                  <h2 className="font-bold">{card.title}</h2>
                  <p>{card.body}</p>
                </div>
              ))}
            </div>
          )}
        </Await>
      </Suspense>
```

</p>
