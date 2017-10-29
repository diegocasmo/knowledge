# Improving Application Performance and Responsiveness on the Client

Performance and responsiveness on client side applications are very important topics which usually don't receive much attention until they are an obvious problem. At [Ubiqua](http://www.ubiqua.me/), as we began to increment the number of features our product offered, we started to experience the following problems:

- Application reloaded too often in mobile devices.
- Application took too long to reload.
- UI thread performed heavy computations which resulted in bad UX.
- Overall application performance was poor.

In this blogpost, I'll show some of the techniques we used to enhance the performance and responsiveness of our application.

### Measure Then Act
The very first thing we have to make sure we do before attempting to fix anything, is to measure what we believe is causing the application to perform poorly. Chrome DevTools offer a variety of tools which facilitate this:

- [Collect JavaScript CPU Profile](https://developer.chrome.com/devtools/docs/cpu-profiling): CPU profiles help to visualize where the execution time is spent in the JavaScript that is being run. Collecting JavaScript CPU profile was critical for us to replicate user actions we already knew were performing slow. We identified methods executed during those user actions which were taking too long and then optimized them. After these methods were optimized, we collected another CPU profile by executing the same previous user actions and we measured if the execution time had decreased in comparison with the first measurements. The console API method ``console.profile([label])`` can turn out to be helpful here as it allows you to measure user actions from specific places in the code and make comparisons.

- [Take Heap Snapshot](https://developer.chrome.com/devtools/docs/heap-profiling): A heap snapshot profile shows memory distribution among JavaScript objects and related DOM nodes.
A key concept to understand when taking heap snapshots is the difference between `shallow size` and `retained size` of a JavaScript object. Shallow size refers to the size of memory that is held by the object itself, while retained size is the size of memory that is freed once the object is garbage collected. When taking heap snapshots, we payed special attention to objects which had a large retained size and thus prevented other objects from being freed from memory. We made a list of these objects and proceeded to re-factor them to only initialize other objects/dependencies when it was strictly needed (if it was needed at all). After that, we took comparison heap snapshots to check if the in memory object count was reducing or incrementing.

- [Timeline Analysis](https://developer.chrome.com/devtools/docs/timeline): The Timeline panel records and analyzes all the activity in an application as it runs. Timeline analysis was convenient for discovering what was making our application take so long when reloading. We learned that is was garbage collection and then proceeded to take heap snapshots to further investigate the issue.

### Freeing up the UI Thread
To battle against the UI thread being blocked by heavy computations we decided to use [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers). A Web Worker provides an API to delegate work to a different thread such that the main UI thread is free to receive user interaction. An important observation to make when using Web Workers is that a new variable has to be added to the time taking to execute a script: `communication`. In addition to the time taken by the script to execute, we must add the time it takes for the application to send the message to the Web Worker, and the time it takes to the Web Worker to send the result back to the application. Because of this, when working with Web Workers always make sure to only pass and return from the Web Worker the information that is needed.

### Faster Rendering
Modifying the DOM through JavaScript is expensive and should only be done when it is a must. We were able to make several performance improvements when the DOM had to be updated by doing the following:

- Use small view components: When data changes small parts of the DOM have to be re-rendered as opposed to re-rendering large parts of it.
- Map child views of a collection and then append to DOM instead of appending each child view individually.
- [Using `setTimeout` of `0` to render non high priority content](http://stackoverflow.com/a/779785): Give the browser a chance to finish doing some non-JavaScript things that have been waiting to finish before attempting to execute this new piece of JavaScript. In other words, it re-queues the new JavaScript at the end of the execution queue.

### Conclusion
In the past months we have learned a lot about improving client side application performance, and many of the techniques described above have now become implicit in our development workflow. Application performance and responsiveness should always be a high priority task in your team, as it directly relates with the main stakeholder of your application: the user.
