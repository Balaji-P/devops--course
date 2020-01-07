# Why Does Jenkins X Need Kubernetes, And Vice Versa
or
# How Jenkins X Makes Kubernetes More Valuable and Vice Versa

We built Jenkins X on top of Kubernetes, and only Kubernetes. Why did we do that? Why would we limit the scope to a single platform? Why do we need Kubernetes, and why does Kubernetes need Jenkins X?

We wanted to build a new CI/CD platform that would be able to meet today's challenges. It's not only about bashing some code, but rather about figuring out how to make it easy to use, how to make it scalable, how to make it fault tolerant, and so on. Building a platform that meets today's needs can take years, unless we take "shortcuts" by leveraging things that are already out there and that can help us accomplish our goals. One of those is Kubernetes. It is a platform that provides all the building blocks one can use to build another platform. That is its design. The "Kubernetes mission" is not to be something that end-users use. Instead, it is a platform designed to build platforms. It allows others to focus on what matters, while providing the features that are required by all, but not differentiators. Being fault-tolerant, highly-available, scalable, etc. is not what differentiates some from others. It is a norm. It is what is assumed today. Having those things is not an advantage, not having them is a suicide. Yet, building those features is a daunting and time consuming endevour that, more often than not, fails. That's where Kubernetes jumps in. It solves those, and many other problems, thus allowing us to focus on what differentiates us from others. Jenkins X would never be what it is, without Kubernetes. We needed Kubernetes, but Kubernetes needs us as well.

Kubernetes is complex. It's much more complex to understand Kubernetes and to operate it than any other "traditional" platform. That, by itself, scares many people. The time one needs to spend to learn basic Kubernetes operations is huge by itself, and yet it is only the begining. We still need to figure how the ecosystem that surrounds it. We need to make a lot of choices. We need to pick the right tools. We need to assemble them into a meaningful platform that fullfills all our needs. How are we going to build container images? How are we going to package and deploy our applications? Where are we going to store the information about the cluster and the environments? There are many questions that each of us needs to ask, and trying to answer each of them quickly leads us to myriad of solutions, without the obvious answer. That's where Jenkins X jumps in. It simplifies the choices. It provides opinions based on the collective experience of the community. It guides users through the maze of the Kubernetes ecosystem. It provides opinions how to manage all steps in the lifecycle of our applications. It makes Kubernetes boring. It makes it useful for all, no matter how much we understand the inner intricacies of the ecosystem.

Jenkins X needed Kubernetes so that the community could focus on solving new problems. Kubernetes needs Jenkins X to guide people through the complexities of its ecosystem.

# What Is Jenkins X?

To understand the **intricacies and inner workings** of Jenkins X, we need to understand Kubernetes. However, you do not need to understand Kubernetes to **use Jenkins X**. That is one of the main contributions of the project. Jenkins X allows us to harness the power of Kubernetes without spending an eternity learning the ever-growing list of the things Kubernetes does. Jenkins X helps us by simplifying complex processes into concepts that can be adopted quickly and without spending months in trying to figure out "the right way to do stuff." It helps by removing and simplifying some of the problems caused by the overall complexity of Kubernetes and its ecosystem. If you are indeed a Kubernetes ninja, you will appreciate all the effort put into Jenkins X. If you're not, you will be able to jump right in and harness the power of Kubernetes without ripping your hair out of frustration caused by Kubernetes complexity.

I'll skip telling you that Kubernetes is a container orchestrator, how it manages our deployments, and how it took over the world by storm. You hopefully already know all that. Instead, I'll define Kubernetes as a platform to rule them all. Today, most software vendors are building their next generation of software to be Kubernetes-native or, at least, to work better inside it. A whole ecosystem is emerging and treating Kubernetes as a blank canvas. As a result, new tools are being added on a daily basis, and it is becoming evident that Kubernetes offers near-limitless possibilities. However, with that comes increased complexity. It is harder than ever to choose which tools to use. How are we going to develop our applications? How are we going to manage different environments? How are we going to package our applications? Which process are we going to apply for application lifecycles? And so on and so forth. Assembling a Kubernetes cluster with all the tools and processes takes time and learning how to use what we assembled feels like a never-ending story. Jenkins X aims to remove those and other obstacles.

Jenkins X is opinionated. It defines many aspects of the software development lifecycle, and it makes decisions for us. It tells us what to do and how. It is like a tour guide on your vacation that shows you where to go, what to look at, when to take a photo, and when it's time to take a break. At the same time, it is flexible and allows power users to tweak it to fit their own needs.

The real power behind Jenkins X is the process, the selection of tools, and the glue that wraps everything into one cohesive unit that is easy to learn and use. We (people working in the software industry) tend to reinvent the wheel all the time. We spend countless hours trying to figure out how to develop our applications faster and how to have a local environment that is as close to production as possible. We dedicate time searching for tools that will allow us to package and deploy our applications more efficiently. We design the steps that form a continuous delivery pipeline. We write scripts that automate repetitive tasks. And yet, we cannot escape the feeling that we are likely reinventing things that were already done by others. Jenkins X is designed to help us with those decisions, and it helps us to pick the right tools for a job. It is a collection of the industry's best practices. In some cases, Jenkins X is the one defining those practices, while in others, it helps us in adopting those made by others.

If we are about to start working on a new project, Jenkins X will create the structure and the required files. If we need a Kubernetes cluster with all the tools selected, installed, and configured, Jenkins X will do that. If we need to create Git repositories, set webhooks, and create continuous delivery pipelines, all we need to do is execute a single `jx` command. The list of what Jenkins X does is vast, and it grows every day.