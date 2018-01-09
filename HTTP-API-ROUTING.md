# Pecan

Pecan is default Neutron HTTP API routing engine since Pike.

```
# ref: https://pecan.readthedocs.io/en/latest/

Pecan was created to fill a void in the Python web-framework world – a very lightweight framework that provides object-dispatch style routing. Pecan does not aim to be a “full stack” framework, and therefore includes no out of the box support for things like sessions or databases (although tutorials are included for integrating these yourself in just a few lines of code). Pecan instead focuses on HTTP itself.

# ref: http://pecan.readthedocs.io/en/latest/routing.html

Pecan uses a routing strategy known as object-dispatch to map an HTTP request to a controller, and then the method to call. Object-dispatch begins by splitting the path into a list of components and then walking an object path, starting at the root controller. You can imagine your application’s controllers as a tree of objects (branches of the object tree map directly to URL paths).
```

---

# Routes

Routes is default Neutron HTTP API routing engine before Pike.

```
# ref: http://routes.readthedocs.io/en/latest/

Routes is a Python re-implementation of the Rails routes system for mapping URLs to application actions, and conversely to generate URLs. Routes makes it easy to create pretty and concise URLs that are RESTful with little effort.
```
