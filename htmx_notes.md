# HTMX:

---

- Get the examples of htmx + flask from [here](https://github.com/Konfuzian/htmx-examples-with-flask).

- Htmx has `<element hx-VERB="/path">` syntax, the VERB /path will be requested on server and server will return `<html>`  as response. Contents of `<html>` response will replace the innerHTML of element **if the request responded in a 200**.

   

  ### Working of `hx-swap-oob`:

  ---

  - Lets say htmx requested some data from client and targeted to an element by specifying a swaping method. But our goal is to swap multiple nodes at different locations for same request, the solution is to send a response with those extra elements (html tags) each wrapped inside the element to replace with and `hx-swap-oob` (swaping method) and `id` attribute to target the element, now client (htmx) will swap each extra element which has `hx-swap-oob` and place tham to given `id` with specified swapping method.
  - However, Adding multiple HTML elements to a single request can become an  overwhelming architectural task on the server. In addition, extra HTML  elements may need further resources from database calls, third-party  services, or other dependencies.
  - So, to avoid out-of-band swaps use `HX-Trigger` http response header which will force all elements subscribed to that event to refresh themselves. [source](https://www.jetbrains.com/guide/dotnet/tutorials/htmx-aspnetcore/out-of-band-swaps/)
  
  
  
  ### Working of request indicators:
  
  The `htmx-indicator` class is defined so that the opacity of any element with this class is 0 by default, making it invisible but present in the DOM.
  
  When htmx issues a request, it will put a `htmx-request` class onto an element (either the requesting element or another element, if specified).  The `htmx-request` class will cause a child element with the `htmx-indicator` class on it to transition to an opacity of 1, showing the indicator.
  
  If you want the `htmx-request` class added to a different element, you can use the [hx-indicator](https://htmx.org/attributes/hx-indicator/) attribute with a CSS selector to do so:
  
  
  
  ### Little CSS selector trick:
  
  **".foo .bar"** selects the element with class "bar" which is inside the class "foo". **".foo.bar"** selects the element which has bot class "foo" and "bar" are applied.