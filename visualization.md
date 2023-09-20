# Using Data Visualization collection

Let's explore Data Visualization collection for now.

Click on `Visualize Bar Chart` request and click on `Body` request to check what will be sent.

Body use `{{$randomInt}}` that is one of some [Dynamic variables](https://learning.postman.com/docs/writing-scripts/script-references/variables-list/) Postman resource.

![Bar Chart visualization request body](images/visualization_bar_chart_request_body.png "Bar Chart visualization request body")

Now click on `Tests` to check how visualization is configured. It looks like this:

![Bar Chart visualization tests](images/visualization_bar_chart_tests.png "Bar Chart visualization tests")

At the end we find visualizer command `pm.visualizer.set( ... );` that contains two parameters: visualization template and visualization data.

Now click on `Send` button to get data response:

![Bar Chart visualization response body](images/visualization_bar_chart_response_body.png "Bar Chart visualization response body")

Response will be presented on `Pretty` default format.

Click on `Visualize` button to see visualization mode. Result looks like this:

![Bar Chart visualization](images/visualization_bar_chart_request_visualize.png "Bar Chart visualization")

Expected result is a chart with city A, B, C and D bars.

Now, let's check `Visualize Table` and `Visualize Map` to check other visualization options.
