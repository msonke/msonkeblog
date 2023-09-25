# Current Problems with the Serverless Architecture from a Logical Business Perspective

## Enter Serverless

When I first started heavily researching tech, one of the buzzwords that keep showing up is the concept of "Serverless", which is a completely misleading term.  It is natural to believe that "Serverless" is actually what it claims to be, lacking servers, but in reality it is a paradigm shift in the era of cloud computing.  

The concept is that instead of having to manage cloud servers manually, including all the maintenance, networking, and infrastructure involved in setting up servers is abstracted away in favor of writing short functions that can be executed on demand instead of having underutilized compute on a persistant server.  Anyone familiar with the concept knows all too well that "Serverless" does not actually mean a lack of servers, the code is still served from servers, except you don't have to deal with the pain of configuring and updating the servers yourself, theoretically leaving you more time to spend on the application code itself.

However, on a personal level the more I have tried to integrate a "Serverless" workflow, I often find that these are by and large ideals, and in practice while certain tasks may become easier, by and large "Serverless" as a paradigm can create many new problems, many of which are detrimental to the development workflow and even the ability to run a profitable business.

Don't be mislead, "Serverless" still does have a place and absolutely has benefits to developers and managers, alike.  The advantages of this "Serverless" paradigm have, however, been largely overblown, and in many case can be a huge mistake in actual practice.  Ultimately the idea of "Serverless" being an DevOps silver bullet should be quickly dismissed. 

## Problems

### Complexity

The first issue that faces any developer trying to incorporate this paradigm is added complexity.  Not only does this favor a functional programming approach which can at times be limiting, at the very least it most always creates extra lines of code and more logic.  The difference between being able to make calculations on a server without a vast amount of API calls and simply writing an inline function most assuredly adds technical debt.

Over time it does become easier, like anything else, to write "Serverless" code.  Even if this is the case, you will <em>always</em> be adding technical overhead to the development.  Granted, essentially you are frontloading this work so that you can save time maintaining servers in the end, and that definitely has value.  In certain scenarios, this is actually the ideal, especially with small teams that don't have a dedicated operations team.  As the site grows, and most sites will, the cognitive load and technical debt grows exponentially.  The business logic most times will not change, the function needed to process something using a traditionally managed server infrastucture usually closely mimics the logic that is implemented in serverless.  What you've added however, is essentially another networking layer between your core logic and application.

Each time you call a serverless function, an API call is made essentially to it's own separate server.  For example, if I had a traditional structure of a front-end and back-end API, in essense you are setting up two servers that even when adding auto-scaling groups only has a single endpoint for each.  Any time you use a "Serverless" function you are adding an endpoint.  For simple applications, this is fine.  As your app begins to grow, the number of endpoints grows exponentially.  Each function needs additional boilerplate code in order to complete an API call, rather than running the compute within the app.

As troublesome as this is, where it really loses it's advantage is in terms of debugging.  Not only are you concerned with the application simply completing your code correctly as normal, you now need to make sure that the connection gateway, cors, AMI access policies, and security are also working correctly.  If not, each of these factors require additional setup and troublshooting.  Very quickly a project that under a traditional development environment would be able to be handled by a smaller staff now takes an ever growing amount of time to deal with the additional problems that a pure "Serverless" infrastructure faces.

What's worse, cloud providers seem to be adding to this complexity.  Anyone who has had to try to decipher the laundry list of AWS services can attest to this.  Problems with taxonomy, deprication, and vendor lock-in are rampant in the cloud computing space.  It is increasingly adding to the overhead of incorporating "Serverless" into your workflow.  Not only do you need to know the specifics of any given technology, but you also need to know the proprietary API calls, documentation, and vendor-specific quirks of a cloud service that many times, the developer already unserstands, but now needs to learn a highly-specific (and non-transferrable) set of skills.  "Serverless" many times becomes a victim of one of the problems it is trying to solve, simplifying one problem to make others much more complex. 

### Cumbersome

Admittedly, I have a slight obsession with efficiency, both in code and workflow.  The stack I chose helps me develop quality applications at a high rate of speed, allowing me to spend less time on trivial tasks and be able to produce a quality product incredibly fast.  Many developers can attest to the pure joy that can come from having a true "flow" to your workflow.

All of this goes out the window when you incorporate "Serverless".

The CLI workflow that allows you to quickly get feedback from your functions' business logic in order to evaluate it's usage properly isn't available to use anymore, essentially removing decades of optimization.  It's not as simple as running a local webserver to see the product as you work on it, as each function now needs to be deployed individually rather than as a whole package.

Essentially, this slows your workflow to a screeching halt.  There is nothing worse than having to deal with the process of getting the code to run at all, and then ensuring business logic is correct.  It is completely counter-intuitive. In addition, while there are tools like CDK and frameworks like AWS SAM and SST, eventually and inevitably there will be times you will be forced to use the GUI web interface of a cloud providerin order to troubleshoot and configure.  Every vim zealot just groaned in pain, myself included.

As mentioned before, there are tools that can help alleviate some of the pain, including being able to run a quasi-local development environment, but many of the struggles still remain, and at the very least it is inevitable that incorporating "Serverless" will slow your development speed.

### Expensive

This is the true issue with "Serverless", and why it is truly misunderstood.  At full load, an application that needs compute 24/7 will be as much as twice(!) as expensive as a regular cloud server.  Why is is that this is being marketed as a cheaper solution?  My background is in marketing, and I can see through the facade quite easily.  The only real time that a pure "Serverless" approach is cheaper, is with very low traffic and workloads.  If you intend to build a proof of concept or pet-project application, and can handle the additional logic overhead, then "Serverless" most likely <em>will</em> be cheaper.  It's only common sense that under the model they sell that "only pay for what you use" would save you money if you don't use a lot of resources.  Indeed, for small apps it is cheaper.

The minute you see decent traffic, it becomes not only more expensive, but often much more expensive.  This is fantastic for the cloud providers, as they can essentially provide the same compute service as before, but charge drastically more for.  As a business owner or manager, your profit margin now belongs to the cloud provider, especially when you consider vendor lock-in.  Under these circumstances, it would have been much more cost effective and fast to simply stick to a traditional server architecture.

Even the Development environment has costs associated with it, and if not careful, a "Serverless" function that contains an infinite loop can be an <em>EXTREMELY</em> expensive mistake.

Personally, I would rather pay slightly more for a service up front in exchange for development speed and significant cost savings at scale.  Clearly this isn't the right approach for <em>every</em> application, but the majority of times the goal is to grow, and as one can clearly see, the more you grow with "Serverless", the more you're constrained.

## Solution

As with most topics in the development world, there are many ways to do any given task, and most of the decisions made are situational and sometimes subjective and based on personal preference.  Clearly there are debates over tech stacks, paradigms, and new concepts such as WASM seemingly on a daily basis, and "Serverless" is no differnt.  Although there are many pros and cons, it is important to know that "Serverless" does have a use case, and it absolutely does have it's place in the current cloud environment.

The largest benefit to "Serverless" is that it solves a problem that Operations is constantly trying to solve, scaleability.  When compared to a traditional infrastructure, it is more difficult to manage the scaling up and down of server instances, even using tools like auto-scaling groups.  Ops has seemingly been going through a revolution, where the trend is to "shift left", incorporating the development team in more of the operations process, helping to alleviate the complexity of maintaining uptime of any application with the ability to meet it's needs without performance degredation.  This move has largely made it easier to manage infrastructure and when done right, can greatly speed the time of deployment, allowing for business managers to rollout features at a rapidly increasing rate.  By and large this is a welcome change, but how can you incorporate the benefits of a "Serverless" architecture while avoiding the necessary pain involved with it?

The answer is simple, only use "Serverless" when it makes sense, and understand the limitations of "Serverless" tech before commiting to it.  There are benefits to running a traditional server as opposed to "Serverless" as well, and while every scenario is different, most times it is better to start with a traditional infrastructure and add "Serverless" only when it makes sense to do so.

Tools like Next.js have made full-stack deployment incredibly fast.  In most cases when launching a product, the stack that can provide some sort of Minimum Viable Product(MVP) is the correct one.  It will be much better for your application in almost any circumstance to start with a traditional infrastructure, while adding "Serverless" API calls for heavily intensive compute tasks.  The functions that one decides to be "Serverless" then are more deliberate, and by deliberately choosing which functions to offload to a worker can not only provide scaleability, but can also increase performance and avoid the excessive complexity that comes with a purely "Serverless" architecture.

In addition, you are able to easily leverage the advantages of a traditional server setup, including warm starts and more understandable and maintainable code, a highly undervalued benefit to the traditional method.  Even better, you are able to leverage languages such as Erlang/Elixir, which can be a breeze to construct heavily fault-tolerant real-time applications, which bring additional benefits that cannot be replicated by pure "Serverless".

By using this approach, you will be incorporating the best of each technology, balancing development speed, operational cost, performance, and maintainability, simply by realizing that "Serverless" is not the one-size-fits-all solution it is sold as.

## Conclusion

At the end of the day, the best tools are the ones that help you achieve your goals in the most effient way possible, and "Serverless" is a part of that journey.  I would hope any developer out there reading this can see that "Serverless" should be used only when needed, and despite cloud providers suggesting that everything be moved to a "Serverless" model, that is rarely the most effective solution for anyone but the ones who sell cloud services. 
