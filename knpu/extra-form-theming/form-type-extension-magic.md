# Form Type Extension Magic

The only way to make an option valid for a field is basically to hack into the core class and add this option. For example, fun fact is a text type. I can open the text type class. If I went down here, I could actually hack in the help option and then I can actually do some other stuff to make this work but that's not going to work because I can't actually hack into this core class. That's where something really cool called a form type extension comes into place. Form type extension is a plugin to Symfony form field system. It allows you to modify any field in the system. For example, if you wanted to ... I don't know ... add a new attribute to literally every single field on your entire site, you could do that with a form type extension. Let me show you what I mean.

Instead of your form directory, lets go new directory called type extension and then new [inaudible 00:01:28] class called help form extension. The goal of this class is going to be to allow for a help option to be passed to each field and to turn that help option into a help view variable. To do that, this needs to extend an abstract type extension class. Now, I'll go to code generate. Is it code generate? Yeah. I'll go to code generate or command + and select implement methods because this requires us to have this 1 get extended type. Now, the idea of the form extensions is that you can create a form extension that will modify every single field on the system or you could create a form extension that will only modify 1 type. For example, you could just modify file upload fields but since we want to modify every field on the system and since every field on the system ultimately extends the form type class, we're going to return that. return FormType::class. Now, we'll modify everything.

Now here's the cool thing about these classes. They have all same functions as a normal form class like [inaudible 00:02:50] form and configure options. The difference is that whatever modifications we make to this class is going to literally effect every single field everywhere. For example, go back to code generate or command + and override the build view function. When we're done setting this up, every single field when it's turned into a form view object, it's actually also going to call this method to figure out what variables it should have available to it. For example, we can do something like view which is whatever field we are currently working on, >vars['help'] and set it to 'TURTLES'. This is a fully functional valid form type extension.

To get integrated into Symfony, you guys can probably guess, we need to register as a service and app config service that [inaudible 00:04:01]. Let's call it app.form.help_form_extension. Set it's class to help form extension. I'm going to autowire it even though it doesn't have any [augments 00:04:10] right now. The key ... the way that you tell symfony to use your form [inaudible 00:04:15] extension when it's rendering the fields, is by adding a tag. The tag is called form.type_extension. You also need to pass it an extended type option and this should equal whatever you are returning from your get extended type. FormType::class will actually be this long string in the use [statement 00:04:45] up here. We can copy that and paste it down here. All right, that's it. Refresher page. To see if this works, temporarily go back into genus form type. Let's [comment 00:05:03] out our help option because that still won't work yet. If you refresh the page, you'll see that every single field now has a turtles help variable except for the 1 field where we actually override it in the form template and that makes sense.

Now, [uncomment 00:05:31] that help option and the question is how do we make this help option valid because right now, it's still now a valid option. The way to do that is in your form type extension, go to command generate or ... go to code generate or command + and override another method configure options. All you need to do here is say resolver>set to fault['help']null. Just by doing that, you are now allowed to have a help option on every single field. It also means that when build view is called, this options array will have a help key in it. All we need to say is if options ['help'] then set the bars variable to options ['help'] and that takes care of it. Wow, consider yourself very, very dangerous.