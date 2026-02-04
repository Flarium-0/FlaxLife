## Better blending between animations

In 3D games, animations are arguably one of the most important aspect. However, it is almost impossible to create *and manage* hundreds of different animations for *each and every action of a charcter.* So, we use something called animation blending - blending between two animations to create a new one.

In Flax Engine, we are provided with a very robust system of Animations, very easy to use but extremely powerful in the hands of a veteran.
The simplest blending is called linear blending where a value of 0 means the character will use the first animation and vice versa. The formula to express it is written as:
$$Pose_{result} = (1 - t) \cdot Pose_{A} + t \cdot Pose_{B}$$

Flax provides linear Blending between animations by default using the `Blend` node:

<img width="279" height="225" alt="image" src="https://github.com/user-attachments/assets/5fce741f-ef49-42c7-a730-17896c1516dc" />

Using the blend node, we can easily create simple Animation blending, like blending between an `Idle` pose and a `Walk` pose:

![anim-blending](https://github.com/user-attachments/assets/74db4907-9460-44b7-a0ae-f0389c381e95)

However, this simple linear blending cannot properly blend between two animations of different lengths.

![bad blending](https://github.com/user-attachments/assets/78dc6c29-fc82-482e-bd77-5d40ff70ca76)

For this, we use a more sophisticated Blending algorithm that tries to match the lengths of both the animations for a smooth blend transition between animations.

<img width="1403" height="758" alt="TruBlend 2_4_2026 12_47_12 PM" src="https://github.com/user-attachments/assets/8ea74698-0fdd-407c-b97c-2edc80e52fc7" />
