1.used to get the router functions code to controller and make the router smaller 
2.moving the logic into controller male the router slim

how to create a controller 

1.node ace malke:controller --help
2.we give name and also give actions or methods inside of that controller that we want the controller to have

3. we can add options
*)singular->generate controller in singular form
*)resource->generate controller with methods to perform CURD actions on a resource
*)api-->generate resourcefull controller with the "edit" and the "create" methods

node ace make:controller movies index show 

index is for home page and show is for our movies show page

HttpContext is a TypeScript type provided by AdonisJS.

It describes the shape of the context object (ctx) that Adonis passes to routes, controllers, and middleware.

HttpContext includes things like:

request

response

params

view

auth

session
async function handler(ctx: HttpContext) {
  ctx.request
  ctx.response
  ctx.params
}

