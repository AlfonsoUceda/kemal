# Views

You can use ERB-like built-in **ECR** views to render files.

```crystal
get '/:name' do
  render "views/hello.ecr"
end
```

And you should have an `hello.ecr` view. It will have the same context as the method.

```erb
Hello <%= env.params["name"] %>
```

## Using Layouts

You can use **layouts** in Kemal. You should pass a second argument.

```crystal
get '/:name' do
  render "views/subview.ecr", "views/layouts/layout.ecr"
end
```

And you should use `content` variable (like `yield` in Rails) in layout file.

```erb
<html>
<head>
  <title><%= $title %></title>
</head>
<body>
  <%= content %>
</body>
</html>
```

## Using Common Paths

Since Crystal does not allow using variables in macro literals, you need to generate
another *helper macro* to make the code easier to read and write.

```crystal
macro my_renderer(filename)
  render "my/app/view/base/path/#{{{filename}}}.ecr", "my/app/view/base/path/layouts/layout.ecr"
end
```

And now you can use your new renderer.

```crystal
get '/:name' do
  my_renderer "subview"
end
```
