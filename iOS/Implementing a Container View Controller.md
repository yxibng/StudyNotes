# Implementing a Container View Controller

参考文章[Implementing a Container View Controller](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/ImplementingaContainerViewController.html#//apple_ref/doc/uid/TP40007457-CH11-SW1)


### Adding a Child View Controller to Your Content
1. Call the `addChildViewController:` method of your container view controller.

 This method tells UIKit that your container view controller is now managing the view of the child view controller.

2. Add the child’s root view to your container’s view hierarchy.

 Always remember to set the size and position of the child’s frame as part of this process.

3. Add any constraints for managing the size and position of the child’s root view.

4. Call the `didMoveToParentViewController:` method of the child view controller.


需要注意的点：

- 步骤： 建立联系 -> 添加视图 -> 调整布局 -> 通知子控制器

- `addChildViewController: `会主动调用` willMoveToParentViewController:`，所以不用主动调用 ` willMoveToParentViewController:` 了

- `didMoveToParentViewController:`在子控制器的视图被添加到父控制器的视图层级之后，手动调用。

示例：

```
- (void) displayContentController: (UIViewController*) content {
   [self addChildViewController:content];
   content.view.frame = [self frameForContentController];
   [self.view addSubview:self.currentClientView];
   [content didMoveToParentViewController:self];
}
```
### Removing a Child View Controller

1. Call the child’s `willMoveToParentViewController:` method with the value nil.

2. Remove any constraints that you configured with the child’s root view.

3. Remove the child’s root view from your container’s view hierarchy.

4. Call the child’s `removeFromParentViewController` method to finalize the end of the parent-child relationship.

步骤：通知子控制器变更即将发生 -> 移除视图 -> 移除控制器关系

注意的点：

- `removeFromParentViewController` 会主动调用 `didMoveToParentViewController:nil`, 不用手动在调用 `didMoveToParentViewController: `了

示例：

```
- (void) hideContentController: (UIViewController*) content {
   [content willMoveToParentViewController:nil];
   [content.view removeFromSuperview];
   [content removeFromParentViewController];
}
```
### Managing Appearance Updates for Children

添加了子控制器之后，父控制器会将视图出现隐藏相关的消息转发给子控制器，子控制器的下列方法会自动调用：

- `viewWillAppear`
- `viewDidAppear `
- `viewWillDisappear `
- `viewDidDisappear `

如果想手动管理子控制器视图出现及隐藏的消息

1. override the `shouldAutomaticallyForwardAppearanceMethods` method in your container view controller and return NO

	```
	
	- (BOOL) shouldAutomaticallyForwardAppearanceMethods {
	    return NO;
	}
	
	```

2. 视图出现或者隐藏的时候调用 

	- `beginAppearanceTransition:animated:`
	- `endAppearanceTransition`
	
	```
	-(void) viewWillAppear:(BOOL)animated {
	    [self.child beginAppearanceTransition: YES animated: animated];
	}
	 
	-(void) viewDidAppear:(BOOL)animated {
	    [self.child endAppearanceTransition];
	}
	 
	-(void) viewWillDisappear:(BOOL)animated {
	    [self.child beginAppearanceTransition: NO animated: animated];
	}
	 
	-(void) viewDidDisappear:(BOOL)animated {
	    [self.child endAppearanceTransition];
	}
	
	```







