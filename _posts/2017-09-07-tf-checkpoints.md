---
date: Thu Sep 07 2017 16:41:34 -0700 (PDT)
qualitative_time: 
title: Working with TensorFlow checkpoints
previous_teaser: Where turtles go to get their shells
previous_first: true
has_highlighting: true
---
I don't blame you if something happens and you have to load some weights into a model, but the variables are different between the checkpoint and the graph.
Here are three things to do in that situation.

## Exclude variables from being restored
This one is the most common issue I run into, where you've added more stuff to the graph, and there's nothing for it to be restored from.

```
tensorflow.python.framework.errors_impl.NotFoundError: Key Variable not found in checkpoint
	 [[Node: save/RestoreV2 = RestoreV2[dtypes=[DT_FLOAT], _device="/job:localhost/replica:0/task:0/cpu:0"](_arg_save/Const_0_0, save/RestoreV2/tensor_names, save/RestoreV2/shape_and_slices)]]

Caused by op u'save/RestoreV2', defined at:
...
```

To exclude variables, you pass a list of variables to the `tf.train.Saver` constructor.
By default, it tries to figure it out on its own, [using `_all_saveable_objects`](https://github.com/tensorflow/tensorflow/blob/v1.3.0/tensorflow/python/training/saver.py#L1156).
That apparently [combines global variables with "saveable objects"](https://github.com/tensorflow/tensorflow/blob/v1.3.0/tensorflow/python/ops/variables.py#L1181), and I don't know what saveable objects are.
Selecting from global variables works for me.

I usually work with code that doesn't keep track of its own variables, so I have to keep track myself.
Here's what I do:

```py
stuff_that_creates_variables()
model_var_start = len(tf.global_variables())
build_the_model()
model_var_end = len(tf.global_variables())
stuff_that_creates_variables()
save = tf.train.Saver(tf.global_variables()[model_var_start:model_var_end])
```

This assumes that global variables are only ever added to the end of the list.
@carlini has some [code](https://github.com/carlini/nn_robust_attacks/blob/1193c79866482d067d15dbdab61a3b50a6903834/l2_attack.py#L105-L109) that doesn't rely on that.

## Look at what's in the checkpoint and what's in the graph
TensorFlow comes with a module that prints out what's inside a checkpoint.

```sh
python -m tensorflow.python.tools.inspect_checkpoint --file_name=log/model.ckpt-122543
```

You'll see the variable names, types, and shapes.

```
input/init_conv/DW (DT_FLOAT) [3,3,3,16]
```

You can print out what's in the graph and compare it.

```py
for v in tf.global_variables():
    print v.name, v.dtype, v.shape
```

```
input/init_conv/DW:0 <dtype: 'float32_ref'> (3, 3, 3, 16)
```

This step doesn't actually fix anything, but at least you can see what's going on.

## Specify what comes from where
If you don't feel like fiddling with the code that creates the variables to make the names match up, you can tell the Saver how to reconcile the different names with a `BaseSaverBuilder`.

```py
class MungeBuilder(BaseSaverBuilder):
    def get_name_in_checkpoint(self, name_in_graph):
        name_in_checkpoint = name_in_graph.replace('init/', 'input/')
        ...
        return name_in_checkpoint
    def restore_op(self, filename_tensor, saveable, preferred_shard):
        return [io_ops.restore_v2(filename_tensor,
                                  [self.get_name_in_checkpoint(spec.name)],
                                  [spec.slice_spec],
                                  [spec.tensor.dtype])[0] for spec in saveable.specs]
saver = tf.train.Saver(builder=MungeBuilder())
```
