Creating Modules
Hands-on: Try the Reuse Configuration with Modules tutorials.

A module is a container for multiple resources that are used together. You can use modules to create lightweight abstractions, so that you can describe your infrastructure in terms of its architecture, rather than directly in terms of physical objects.

The .tf files in your working directory when you run terraform plan or terraform apply together form the root module. That module may call other modules and connect them together by passing output values from one to input values of another.

To learn how to use modules, see the Modules configuration section. This section is about creating re-usable modules that other configurations can include using module blocks.

Module structure
Re-usable modules are defined using all of the same configuration language concepts we use in root modules. Most commonly, modules use:

Input variables to accept values from the calling module.
Output values to return results to the calling module, which it can then use to populate arguments elsewhere.
Resources to define one or more infrastructure objects that the module will manage.
To define a module, create a new directory for it and place one or more .tf files inside just as you would do for a root module. Terraform can load modules either from local relative paths or from remote repositories; if a module will be re-used by lots of configurations you may wish to place it in its own version control repository.

Modules can also call other modules using a module block, but we recommend keeping the module tree relatively flat and using module composition as an alternative to a deeply-nested tree of modules, because this makes the individual modules easier to re-use in different combinations.

When to write a module
In principle any combination of resources and other constructs can be factored out into a module, but over-using modules can make your overall Terraform configuration harder to understand and maintain, so we recommend moderation.

A good module should raise the level of abstraction by describing a new concept in your architecture that is constructed from resource types offered by providers.

Terraform makes it easier to grow your infrastructure and keep its configuration clean. But, as the infrastructure grows, a single directory becomes hard to manage.

That’s where Terraform modules come in. 

In this post you will learn what Terraform modules are, how to use them, and what problems they solve.

What Is a Terraform Module?
A Terraform module is a collection of standard configuration files in a dedicated directory. Terraform modules encapsulate groups of resources dedicated to one task, reducing the amount of code you have to develop for similar infrastructure components.

Some say that Terraform modules are a way of extending your present Terraform configuration with existing parts of reusable code reducing the amount of code you have to develop for similar infrastructure components. Others say that the Terraform module definition is a single or many .tf files stacked together in their own directory. Both are correct.

Module blocks can also be used to force compliance on other resources—to deploy databases with encrypted disks, for example. By hard-coding the encryption configuration and not exposing it through variables, you’re making sure that every time the module is used, the disks are going to be encrypted. 

A typical module can look like this:

.
├── main.tf
├── outputs.tf
├── README.md
└── variables.tf

As you can see, practically any Terraform configuration is already a module in itself. If you run Terraform in this directory, those configuration files would be considered a root module. It means that this configuration is the base of your operation, a core that you can expand further.

Note: New versions of Terraform will be placed under the BUSL license, but everything created before version 1.5.x stays open-source. OpenTofu is an open-source version of Terraform that will expand on Terraform’s existing concepts and offerings. It is a viable alternative to HashiCorp’s Terraform, being forked from Terraform version 1.5.6. OpenTofu retained all the features and functionalities that had made Terraform popular among developers while also introducing improvements and enhancements. OpenTofu is not going to have its own providers and modules, but it is going to use its own registry for them.

How To Use Terraform Modules
OK, now that you know what a Terraform module is, let’s take a look at a step-by-step process of creating them. So, let’s get started!

1. Declare that you want to use Terraform modules
To use a Terraform module, you have to first declare that you wish to use it in your current configuration. To do this, use the module block and provide the appropriate variable values:

module "terraform_test_module" {
 source  = "spacelift.io/your-org/terraform_test_module"
 version = "1.0.0"
 
 argument_1                     = var.test_1
 argument_2                     = var.test_2
 argument_3                     = var.test_3
}
As you’re adding the variables, there are a few arguments that you should keep an eye on: their source, version, and four meta-arguments.

Sources
Terraforms modules can be stored either locally or remotely. The `source` argument will change depending on their location. For example, if the module you wish to call is stored in a directory named “terraform-test-module” located in the same place as your root module directory, your root configuration would look like this:

module "terraform_test_module" {
 source  = "./terraform-test-module"
[...]
But, if you want to keep the module in a VCS (let’s assume git and GitHub), the source will need to point to the correct repository containing the appropriate version of the module that you’re calling:

module "terraform_test_module" {
 source  = "git@github.com:your-organization/the-repository-name.git?ref=1.0.0"
[...]
Terraform modules can also be stored in so-called registries. Registries are places where you can find modules published by fellow Terraform users, or where you store the ones you have created—either privately, for your company/yourself, or publicly, for everyone to enjoy. A Terraform module called from a registry might look like this:

module "terraform_test_module" {
 source  = "<registry address/<organization>/<provider>/<module name>"
 version = "1.0.0"
[...]
Please note that the URL scheme depends on the registry provider. For example, imagine that you wish to include a VPC module stored in the Spacelift registry in your configuration:

module "vpc" {
 source  = "spacelift.io/your-organization/vpc-module-name/aws"
 version = "1.0.0"
[...]
As you’ve probably noticed, those two examples are a bit different from one another. A big advantage of the Spacelift’s registry is that it provides an example for each module. This means that you can check whether the source argument in your configuration is correct. Additionally, you can grab the entire example and use it as a template, filling in the blanks, and starting your work right away!

Versions
Versioning enables you to control what module changes should be introduced into your infrastructure. Why is it useful? It helps prevent the damage of your infrastructure caused by unpredictable updates or faulty code. You probably know from experience that any update of software, packages, or applications on a production server can go wrong. This can also happen to Terraform modules, if they’re not kept under control.

The syntax for version constraints is simple. Simply put, a version is a string containing one or more conditions. 

The first part is an operator:

“=” (or no operator) means “Only one version—this specific version.”
“!=” translates to “Other versions are fine, except this one here”
“>, >=, <, <=” are used for comparisons. For example, “Use any version newer than 1.0.0, but older than 1.1.0”
“~>” is quite interesting. This operator allows only the rightmost part of the version number to increment. In other words, “~>2.6.0” would mean that you wish to use the newest patch version of the module (2.6.<anything>), but not the newer minor or major versions (2.7.0 or above).
The second part of the syntax is the version number. Although it isn’t absolutely required (except when using registries), it’s best to stick to the Semantic Versioning convention—it’s easy to understand and very transparent. One look at the version, and you know where you are.

As you might have already noticed, there is an exception to the “keep things versioned” rule—in two of the examples above, the version argument is defined. In one, it’s not. The reason for this is that Terraform considers modules loaded from the same source repository as being of the equal version to the caller. Local modules, by the way, need no version constraints whatsoever.

Meta-Arguments
Meta-arguments are special arguments that change the behavior of Terraform when parsing the declared module. Currently, there are four of them in active use:

“Count” and “for_each” are used to create multiple instances of the same module or resource. More on this: Terraform Count vs. For_Each Meta-Argument: When to Use Them
“providers”— this meta-argument allows you to directly declare which provider the module should use for its operation. This comes in handy if you have two cloud service accounts and you want to create resources bound to the secondary. In order to achieve that, you need to properly design your Terraform module so that it takes the provider as an argument and passes it along, like this:
provider "aws" {
 alias  = "frankfurt"
 region = "eu-central-1"
}
 
module "example" {
[...]
 
 providers = {
   aws.nested_provider_alias = aws.frankfurt
 }
}
“depends_on”—usually, Terraform handles implicit dependencies quite well, but sometimes they aren’t enough. If you need to declare that something should be created before something else, use this meta-argument to define an explicit boundary between resources or modules.

2. Declare Module Outputs
Sometimes you might need to use the values that are available in the already created resources. A Terraform module completely encapsulates those resources, and here’s how they can be accessed. 

First, declare in your Terraform module that the selected value should be available as an output:

output "random_string" {
 value       = aws_example_resource.example_device.random_string
 description = "A random string from an example resource on AWS."
}
Then, call this value like this:

resource "example_resource" "example" {
 [...]
 
 random_string = module.example.random_string
}
The module outputs are not passed on from the resources by default—so if you don’t declare an output, it won’t exist.

3. Taint Terraform Modules
Sometimes you might need to recreate some or even all component parts of the Terraform module. As there is no way of flagging an entire module that needs to be recreated, you will need to manually taint every resource that was created by the module. While it may seem like a downside, it is actually a feature that offers additional protection against human error. What’s more, you will rarely need to taint an entire Terraform module.

To taint the resources that were created by the example module presented above, try this: 

terraform taint module.example.aws_example_resource.example_something

4. Test Modules
Whether a blessing or a curse, Terraform’s code is still code, and should be properly tested. It is absolutely crucial. But when done manually, it can be quite a chore. Luckily, Spacelift, alongside other upgrades and extensions, provides automated module testing and you will absolutely love it. 

Terraform modules managed by the Spacelift registry get tested every time you push a new commit. They are applied, deleted, and you get all of the details on the run. If something fails along the way, the entire test fails, too. If everything goes smoothly, the test is successful. The entire process is automatic, but if you feel like playing around with it, you can even write your own tests, such as this one:

provider "aws" {
 region = "eu-central-1"
}
 
resource "random_string" "this" {
 length  = 16
 upper   = false
 number  = false
 special = false
}
 
resource "aws_vpc" "test" {
 cidr_block = "10.10.0.0/16"
}
 
module "test" {
 source = "../"
 
 cidr_blocks = {
   eu-central-1a = "10.10.0.0/24"
 }
 
 environment = "test-${random_string.this.result}"
 name        = "test-${random_string.this.result}"
 vpc_id      = aws_vpc.test.id
}
Customizable automated testing—what’s not to love here?

What Problems Do Terraform Modules Solve?
Alright, now that you know how to use Terraform modules and what to keep an eye on when doing it, let’s see what problems Terraform modules solve.

1. Code Repetition
As your Terraform infrastructure grows in scale, the code will need to do so as well. Copy-pasting an entire stack of code whenever you need more than a single instance of something isn’t scalable or efficient. It’s a waste of many things, time in particular.

2. Lack of Code Clarity
Copy-pasting isn’t clean either. Good code is readable, and great code documents itself. A modular approach addresses all those pesky details. A configuration of connecting modules dedicated to particular tasks is much easier to read and understand. 

3. Lack of Compliance
When you create a Terraform module in compliance with appropriate standards and best practices, whenever you reuse it, it follows the same right pattern. No matter if it’s encryption, redundancy, or lifecycle policies—practices configured inside the module will be enforced, so that you won’t have to do it again personally.

4. Human Error
When you copy-paste or create a group of resources from scratch, it’s easy to make a mistake, like rewriting or renaming something. To make sure that doesn’t happen, create a single Terraform module, test it, then use it in multiple places to check whether all of the elements are correct. It’s easier to check and test what you type into a single block than scroll, jumping from one place to another, and so on. 

As you can see, there are a lot of advantages to using Terraform modules, so long as they aren’t overused. Find the right balance and keep it. 

Terraform Modules Best Practices
Each Terraform module should live in its own repository and versioning should be leveraged.
Minimal structure: main.tf, variables.tf, outputs.tf.
Each Terraform module should have examples inside of them.
Use input and output variables (outputs can be accessed with module.module_name.output_name).
Used for multiple resources, a single resource module is usually a bad practice.
Use defaults or optionals depending on the data type of your variables.
Use dynamic blocks.
Use ternary operators and take advantage of Terraform built-in functions.
Test your modules.