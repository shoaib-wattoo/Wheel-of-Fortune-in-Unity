## Step 1 : Introduction to Wheel of Fortune

I guess, you already know what Wheel of Fortune is.
If you’re fond of playing **Candy Crush**, you should have already spun that **Daily Booster Wheel** countless times.

**Spin & Win** from **8 Ball Pool** is also quite popular among gamers.
Or,
You might have seen it on some reality TV game shows, where a person spins a wheel to get a prize or a task to win the prize.

**Do you want similar kind of wheel in you game?**

If yes! Then you have come to the right place!
Its quite simple, but you might find a couple of things tiny bit tricky and confusing.
Worry not! I will try to make it simple as I can.
Lets get rolling then..!

## Step 2 : Scene Setup
Let's Setup the Scene.

#### 2.1 Wheel Setup
- Set your wheel in the scene view. Make sure the partitions on the wheel are of same size.
- Set small circle colliders on the edges (you need to create another empty GameObjects to store colliders) and set them as children of your wheel GameObject.
- Now to keep the hierarchy neat & tidy you can create another empty GameObject as parent to the collider objects.

#### 2.2 Arrow Setup
- Im taking a simple black cube to represent an arrow; you can use any of your favorites.
- Add a **BoxCollider2D** & a **HingeJoint2D** component to your Arrow object.

If you have never encountered **HingeJoint** component before, it works as a hinge joint like we use in doors & windows.

Spend some time to play with its properties to get your desired result.

**You can also refer to the Unity Docs for more detailed descriptions:**
- [https://docs.unity3d.com/Manual/class-HingeJoint.html](https://docs.unity3d.com/Manual/class-HingeJoint.html)

We’ve completed the scene setup now.

**It should look something like this:**

![](http://www.theappguruz.com/app/uploads/2016/07/scene-setup2.png)

## Step 3 : Scripting
This time there’s only one script for us to worry about. (That’s a good thing, right?)

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
public class SpinWheel : MonoBehaviour
{
    public List<int> prize;
    public List<AnimationCurve> animationCurves;
    
    private bool spinning;    
    private float anglePerItem;    
    private int randomTime;
    private int itemNumber;
    
    void Start(){
        spinning = false;
        anglePerItem = 360/prize.Count;        
    }
    
    void  Update ()
    {
        if (Input.GetKeyDown (KeyCode.Space) && !spinning) {
        
            randomTime = Random.Range (1, 4);
            itemNumber = Random.Range (0, prize.Count);
            float maxAngle = 360 * randomTime + (itemNumber * anglePerItem);
            
            StartCoroutine (SpinTheWheel (5 * randomTime, maxAngle));
        }
    }
    
    IEnumerator SpinTheWheel (float time, float maxAngle)
    {
        spinning = true;
        
        float timer = 0.0f;        
        float startAngle = transform.eulerAngles.z;        
        maxAngle = maxAngle - startAngle;
        
        int animationCurveNumber = Random.Range (0, animationCurves.Count);
        Debug.Log ("Animation Curve No. : " + animationCurveNumber);
        
        while (timer < time) {
        //to calculate rotation
            float angle = maxAngle * animationCurves [animationCurveNumber].Evaluate (timer / time) ;
            transform.eulerAngles = new Vector3 (0.0f, 0.0f, angle + startAngle);
            timer += Time.deltaTime;
            yield return 0;
        }
        
        transform.eulerAngles = new Vector3 (0.0f, 0.0f, maxAngle + startAngle);
        spinning = false;
            
        Debug.Log ("Prize: " + prize [itemNumber]);//use prize[itemNumnber] as per requirement
    }    
}
```

#### Note
Before I start explaining the code, attach the script to your wheel GameObject so that you can see the effect of code. That will make things easier to understand.

### 3.1 Animation Curves

As you can see in the code I’m using **Animation Curves** to set the wheel in motion. You can set a variety of shapes in these curves to have various kinds of motions for your wheel.

**Animation Curves look like something below:**

![](http://www.theappguruz.com/app/uploads/2016/07/animation-curve.png)

To access the curves, click on the curve in the **inspector** tab.

![](http://www.theappguruz.com/app/uploads/2016/07/animationcurves.png)

Also **DO NOT** forget to set the proper size of prize list.

**Now to Update() method:**
- It contains 2 random numbers and a coroutine call: **SpeenTheWheel()**
- **itemNumber** sets the item which will be under the arrow when the wheel stops.
- **maxAngle** (or finalAngle)denotes what the final angle will be.

[Yes! We will know the result before spinning the wheel. But do not worry; it is still a mystery for our players.]

**Lets check SpinTheWheel() coroutine:**
- At first the timer, startAngle and maxAngle are initialized.
- After that a random animation curve is selected.

**And the while loop inside:**
- The code inside is to calculate angle (rotation in z) for the wheel.
- Here **Evaluate()** of animation curve gives the value of curve for given time.

The spinning logic for our wheel ends here.

## Step 4 : Conclusion

Now it’s up to you how to use the prize on the itemNumber that we get at the end (which we knew from the beginning :) ).

Now you know why we never got that so called **"BIG PRICE"** or **"JACKPOT"** EVER..!!
