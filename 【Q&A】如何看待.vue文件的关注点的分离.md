# Single-File Components

One important thing to note is that separation of concerns is not equal to separation of file types. In modern UI development, we have found that instead of dividing the codebase into three huge layers that interweave with one another, it makes much more sense to divide them into loosely-coupled components and compose them. Inside a component, its template, logic and styles are inherently coupled, and collocating them actually makes the component more cohesive and maintainable.

大意：
最主要的分别要知道：关注点的分离不代表文件类型的分离。在现在的UI开发中，我们发现相比于把html,css,js三者代码库分离成三个大的层次并将其相互交织起来，把它们分别放在互相解耦的组件中并组合起来更合理一些。在组件中，它的template，logic,styles是内部耦合的，并且把他们搭配在一起实际上使得组件更加内聚且更可维护。
