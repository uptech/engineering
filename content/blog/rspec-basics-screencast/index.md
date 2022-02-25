+++
title = "RSpec Basics Screencast"
date = 2017-02-04T09:02:38-08:00
updated = 2017-02-04T09:02:38-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
+++

This is yet another screencast I dug up while cleaning out some of the digital clutter. This one provides a great basic walk through of outside in development despite using a sligtly older version of RSpec. The biggest difference in RSpec versions at this point is the way expections are written. In this screencast it references the old style:

```ruby
Catalog.should_receive(:all)
```

The new style avoids implementations that require monkey patching. This now looks as follows.

```ruby
expect(Catalog).to receive(:all)
```

Despite this small deviation this screencast is still extremly valuable and hopefully helps people get a grasp on outside in development and the basics of RSpec. You should be able to easily adjust for the difference by referencing the [rspec-expectations](https://github.com/rspec/rspec-expectations) documentation.

<video controls="controls">
  <source src="//media.upte.ch/tcb-0003-rspec-basics.m4v" type="video/mp4">
  <source src="//media.upte.ch/tcb-0003-rspec-basics.ogv" type="video/ogg">
  <source src="//media.upte.ch/tcb-0003-rspec-basics.mov" type="video/quicktime">
  Your browser does not support the <code>video</code> element. Please upgrade/switch to a more modern browser that does if you want to be able to view videos.
</video>

<a href="//media.upte.ch/tcb-0003-rspec-basics.m4v" class="btn btn-primary navbar-btn"><i class="fal fa-download"></i> .m4v</a>
<a href="//media.upte.ch/tcb-0003-rspec-basics.ogv" class="btn btn-success navbar-btn"><i class="fal fa-download"></i> .ogv</a>
<a href="//media.upte.ch/tcb-0003-rspec-basics.mov" class="btn btn-info navbar-btn"><i class="fal fa-download"></i> .mov</a>

In this episode we reset the state of code to the point where in episode 2 we should have dropped down and fleshed out the RSpec tests for the controller. This episode covers the very basics of RSpec while we drive out a Rails controller action as part of the feature outlined in episode 2.

<table class="table table-condensed" style="margin-top: 40px;">
	<tr>
		<th>Release Date</th>
		<td>2013-05-15</td>
	</tr>
	<tr>
		<th>Size</th>
		<td>79.7 MB</td>
	</tr>
	<tr>
		<th>Duration</th>
		<td>21 mins</td>
	</tr>
	<tr>
		<th>Resolution</th>
		<td>1920 x 1080</td>
	</tr>
</table>

