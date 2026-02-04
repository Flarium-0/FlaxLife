## Better blending between animations

In 3D games, animations are arguably one of the most important aspect. However, it is almost impossible to create *and manage* hundreds of different animations for *each and every action of a charcter.* So, we use something called animation blending - blending between two animations to create a new one.

In Flax Engine, we are provided with a very robust system of Animations, very easy to use but extremely powerful in the hands of a veteran.
The simplest blending is called linear blending where:
- Alpha = 0 means the character will use **only the first animation.**
- Alpha = 0.5 means the character will use an animation composed of `50%` of the first animation and `50%` of the second animation.
- Alpha = 1 means the character will use **only the second animation.**

The formula to express it is written as:
$$Pose_{result} = (1 - t) \cdot Pose_{A} + t \cdot Pose_{B}$$

Flax provides linear Blending between animations by default using the `Blend` node:

<img width="279" height="225" alt="image" src="https://github.com/user-attachments/assets/5fce741f-ef49-42c7-a730-17896c1516dc" />

Using the blend node, we can easily create simple Animation blending, like blending between an `Idle` pose and a `Walk` pose:

![anim-blending](https://github.com/user-attachments/assets/74db4907-9460-44b7-a0ae-f0389c381e95)

However, this simple linear blending cannot properly blend between two animations of different lengths.

![bad blending](https://github.com/user-attachments/assets/78dc6c29-fc82-482e-bd77-5d40ff70ca76)

For this, we use a more sophisticated Blending algorithm that tries to match the lengths of both the animations for a smooth blend transition between animations. This algorithm is called TruBlend, and I shall walk you, brave developer, across the bridge of Animation blending.

## Enter TruBlend
**TruBlend** is a slightly more sophisticated algorithm than traditional linear Blending. As said earlier, it tries to match the lengths of the animations to be blended, so that, for every one second of time, both the animations advance by the same amount.

### Steps to create TruBlend
We will implement TruBlend in a `Animation Graph Function` for reusability.

- We will accept the lengths of the animations (as `ANIM 1 length` and `ANIM 2 length` respectively) as `Float` and the animation blend factor (here written as **Speed**, which is a value of 0-1) as a `Float`.
- Now to derive the final speeds of the animations, we use this formulae:
> $$ANIM 1 speed = Lerp(1.0, (ANIM 1 Length / ANIM 2 length), Speed)$$
> $$ANIM 2 speed = Lerp((ANIM 2 Length / ANIM 1 length), 1.0, Speed)$$

- The statement `Lerp(1.0, (ANIM 1 Length / ANIM 2 length), Speed)` can be understood as, we first find the ratio of the lengths of ANIM 1 and ANIM 2. If the `length of ANIM 1 > length of ANIM 2`, then their ratio will be `> 1`, meaning that the speed of ANIM 1 should be increased to keep up with ANIM 2. We then perform a lerp between 1.0 and this ratio, with Speed as the factor, this ensures that, the speed will be increased proportionally to the actual requirement of the speed. For instance, if we plan to play only the first animation, we do not need to match its speed to the second and vice versa. Remember that the speed of the second animation is also being modulated in a similiar way.

-  The statement `Lerp((ANIM 2 Length / ANIM 1 length), 1.0, Speed)` can be understood as, we first find the ratio of the lengths of ANIM 2 and ANIM 1 (Opposite of what we did in the previous step). If the `length of ANIM 2 > length of ANIM 1`, then their ratio will be `> 1`, meaning that the speed of ANIM 2 should be increased to keep up with that of ANIM 1. We then perform a lerp between the above ratio and 1.0, with Speed as the factor, this ensures that, the speed will be increased proportionally to the actual requirement of the speed. For example, if we plan to play only the second animation, we do not need to match its speed to the first.

-  As a last step, we simply perform a linear blend between the two animations that we accepted as `ANIM 1` and `ANIM 2` (`Skeleton pose` in local space) and pass it out as another output.

Once the last step is done, our `Animation Graph Function` should look something like this:

<img width="1403" height="758" alt="TruBlend 2_4_2026 12_47_12 PM" src="https://github.com/user-attachments/assets/8ea74698-0fdd-407c-b97c-2edc80e52fc7" />

## Using TruBlend in Animation Graph for smooth blending!
Give yourself a pat, you've singlehandedly solved a Animation problem! But, hold your horses ladies and gentlemen, we still need to learn to use this very beautiful`TruBlend node` in our `Animation graph`!

If you have not set up your `Animation graph` yet, do so using [this tutorial](https://docs.flaxengine.com/manual/animation/tutorials/use-anim-graph.html)

Now, just drag the `TruBlend node` into the `Animation graph` and set it up like this:

<img width="1231" height="790" alt="image" src="https://github.com/user-attachments/assets/7f02f11e-efb0-46c5-b22b-4830f04f36eb" />

Plug the `ANIM 1 speed` into the `Speed` of `Animation 1`, and the `ANIM 2 speed` into the `Speed` of `Animation 2`. Plug the Length of Animation 1 into ANIM 1 length and the Length of Animation 2 into ANIM 2 length.

Done!

Here is the result!. Notice how smoothly it is blending between two animations.

https://github.com/user-attachments/assets/2a584cd0-436b-4b6e-8948-2bca161e7c98

### Note: If, in game you face a problem where animations are not in sync, then limit the `speed` value to a range of `0.1` to `0.9`.

Thanks for being here!


