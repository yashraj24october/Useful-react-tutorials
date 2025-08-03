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
<p>
eg: import { useLoaderData } from 'react-router-dom';<br/><br/>
const data = useLoaderData();<br/><br/>

return (<br/>
<div className="content-list"><br/>
  {<br/>
    data.map((item)=>{<br/>
  return .....
});<br/>
}<br/>
</div><br/>
  )
</p>

<h3>This way is much faster than tradition way in which we load the data in component itself.</h3>
