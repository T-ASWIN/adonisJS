Singleton Service

A class that is instantiated only once in the entire application, and that single instance is shared everywhere.


We create ONE object from the class…
and reuse that same object everywhere.

class CacheService {
  ...
}

const cache = new CacheService()
export default cache


So the values stored inside the singleton stay shared, right?
