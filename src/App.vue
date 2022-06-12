<!-- 1. Animation Basics & CSS Transitions
======================================

-Our goal is to animate   <div class="block"></div>
-When the <button>Animate</button> is clicked

-Steps To Reproduce
-------------------

    -Add data property animatedBlock and set to false

    -Add a click event to button that fires animateBlock() which sets animatedBlock to true
      e.g. <button @click="animateBlock">Animate</button>

    -Add animate class which shoud be added dynamically when animatedBlock is true
      e.g.  <div class="block" :class="{ animate: animatedBlock }"></div>

    -Add .animate{} style to move the block to the left -150px along Xaxis using transform : translateX(-150px)

      e.g. .animate {
                  transform : translateX(150px);
                }

    - Moves the block to the left then to the initial position after 3sec you could say

    -Adding Transition effect
    --------------------------

    -Add transition to the block which is the base class that we want to animate
    
    -Specify the values (CSS-property ,duration in seconds ,effect  )

            .block {
          width: 8rem;
          height: 8rem;
          background-color: #290033;
          margin-bottom: 2rem;
          transition : transform 0.3s ease-out;
        }

        -->




 <!-- 2. Understanding CSS Animations
======================================

-We use  @keyframes nameOfTheAnimation/identifier {}

-Then define in details how the animation should behave in different states expressed in %

-0% - Means the initial state/position

-100% - Means the final state/position


 e.g.   @keyframes slide-scale {

                0% {
                  transform : translateX(0) scale(1);
                }

                70% {
                   transform : translateX(-120px) scale(1.1);
                }

                100% {
                  transform : translateX(-150px) scale(1);
                }
            
             }

-Then we apply the animation by calling it inside the .animate {} css rule

-i.e.
         animation : (identifier) (duration) (ease-function) (forwards/reverse)

-(identifier) - refers to the name of our animation

-(duration) - transition period in milliseconds

-(ease-function) - control how fast the different states should be reached
      ease-in    ; Start slow and end fast

      ease-out ; Start fast and end-slow

-(forwards/reverse) - Tells CSS to keeps the final state as the new state of the animated element

e.g.
          .animate {
                    animation : slide-scale 0.3s ease-out forwards;
                }

N/B ; By default CSS will move the animated element to its initial position if forwards property is not specified


-->




 <!-- 5. Playing CSS Animations with Vue 's help - Theory
======================================

-Lets add a button that toggles a paragrah when it is clicked as follows

      isParaVisible : false

      <div class="container">
          <p v-if="isParaVisible"> Some text to toggle</p>
          <button @click="toggleParagraph">Toggle Paragraph</button>
        </div>

      -And the logic for toggling

        toggleParagraph(){
            this.isParaVisible = !this.isParaVisible;
          },

-Now let's animate with Vue and see how it helps us here

-<transition></transition> is  a built-in component which you can wrap around the element where you want to control appearance and removal animation with vue

-In our case ; The paragraph to be animated
    <transition>
      <p v-if="isParaVisible"> Some text to toggle</p>
    </transition>

-You can wrap multiple elements in a componet or per multiple Vue -apps

-<transition></transition> - MUST ONLY CONTAIN ONE DIRECT CHILD ELEMENT - Though there is an exception which will be covered later in this course

-<transition></transition> - Gives us the following Animating ENTER-LEAVE STATES

-It adds a couple of utility css classes to that element :

--ENTERING/APPEARANCE PHASE
------------------------------

ELEMENT_NOT_MOUNTED => ELEMENT_MOUNTED

    *-enter-from class - Added first
    *-enter-active class -Added at the same time as the enter-from 
    *-enter-to - Added right when the animation finishes

-Vue will find how long that process shd take by analysing these special CSS classes and looking for transition and animations inside of them to then read the duration from them

-Vue will analyse your CSS code for these special CSS class names ; Find out the duration ; Then add  them over the duration ; And this helps you set the styles during the appear entering phase of the elmnt

--LEAVING/REMOVAL PHASE
------------------------------
ELEMENT_NOT_MOUNTED => ELEMENT_MOUNTED => ELEMENT NOT MOUNTED

-The same concept is used during the removal of the element

-Vue adds the following classes when the elemnt is being unmounted :-

   *-leave-from class - Added first
    *-leave-active class -Added at the same time as the leave-from
    *-leave-to - Added right when the animation finishes

-Vue will analyse your CSS code for these special CSS class names ; Find out the duration ; Then add  them over the duration ; And this helps you set the styles during the disappearance / removal phase of the elmnt

-Will only remove the Element from the DOM once the animation is over;

-->




 <!-- 6. Using The <transition></transition> Component  
======================================

-As explained Vue will add certain utility classes ; So let's add them to control how Vue adds the element to the DOM;

-In our case ; When the paragraph is appearing in the DOM for the first time

-The default classes Vue will add are : 
      .v-enter-from {}
      .v-enter-active {}
      .v-enter-to {}

-Now we can define diff styles / properties and we should include one tranition / animation so that Vue is able to read the duration time from these classes 

- .v-enter-from {}  
--------------------------
    Initial state ;
     -Invisible ;
     -Moved above a little bit it's default position;

        .v-enter-from {
          opacity : 0 ;
          transform : translateY(-30px);
        }

- .v-enter-active {}
----------------------
    -This is where we add the transition property to tell Vue to watch for all CSS properties that might be animated i.e opacity and transform 
    -Then the duration - How long this classes shd be added to the element 
    -Easing-function -Controls how fast - start-slow /end-fast ; start-fast/end-slow

      .v-enter-active {
      transition : all 0.3s ease-out;
    }


-   .v-enter-to  {}
--------------------
    Final state ;
       -Visible;
       -Moved to the position of the real element that it should have on the page

        .v-enter-to {
          opacity : 1 ;
          transform : translateY(0);
        }

-We also have the same concept when the element is removed from the 

-The special utility classes Vue will look for are:-

      .v-leave-from {}
      .v-leave-active {}
      .v-leave-to {}

-And uses the same concept / opposite of entering

- .v-leave-from {}
--------------------------
    Initial State;
      -Visible
      -Default Position

        .v-leave-from {
          opacity : 1 ;
          transform : translateY(0);
        }

- .v-leave-active {} - Transition - How the duration is split across CSS properties
--------------------------

    .v-leave-active {
          transition : all 0.3s ease-out;
        }

- .v-leave-to {} - Hiding it Completely
---------------------------------------

    .v-leave-to {
          opacity: 0;
          transform: translateY(-30px);
        }

-->




<!-- 6. CSS Animations with <transition></transition> Component  
======================================

-In this case we don't need 
    .v-enter-from and .v-enter-to

-We only call .v-enter-active {} and call the animation defined in @keyframes

  .v-enter-active{
    animation : slide-scale 0.3s ease-out;
  }
  :N/B - forwards too property is not needed

-We can do exactly the same thing with Leaving; We dont need
  .v-leave-from and .v-leave-to

-We only call   .v-leave-active{  } and call the animation

   .v-leave-active{
    animation : slide-scale 0.3s ease-out;
  }

  :N/B - forwards too property is not needed


-->

<!-- 7. Using Custom CSS Class Names 
======================================

-In cases where we have several <transition></transition> components , Vue will still apply the special utility classes ;
    Rendering :  v-enter-from;.v-enter-active; .v-enter-to
    Removing :  v-leave-from;.v-leave-active; .v-leave-to

-If you have more than one transition components , this might not work because of the conflicts of these classes

-Therefore we can introduce a name property on the <transition></transition> component which will then be used by Vue to find these classes by the custom names

In our case of the paragraph ; That would be 

  e.g. 

    <transition name="para">
      <p v-if="isParaVisible"> Some text to toggle</p>
    </transition>

-And then for our rendering special utility classes , we define them as them as :

  .para-enter-from {
      opacity: 0;
      transform: translateY(-30px);
    }

    .para-enter-active {
      transition : all 0.3s ease-out;
    }

    .para-enter-to {
      opacity : 1;
      transform: translateY(0);
    }

-And then for our detaching/removing special utility classes , we define them as them as :

  .para-leave-from {
      opacity: 0;
      transform: translateY(-30px);
    }

    .para-leave-active {
      transition : all 0.3s ease-out;
    }

    .para-leave-to {
      opacity : 1;
      transform: translateY(0);
    }

-You can also have cases where you are using a third party libraries for animations

-In those cases , then you don't need name="" property in your <transition></transition> component but instead you can use 

    <transition enter-from-class="from_class" enter-active-class="active_class" enter-to-class="to_class" >
        <p v-if="isParaVisible"> Some text to toggle</p>
    </transition>

-And then define property for these classes i.e. from_class ,  active_class ,  to_class

-Therefore instead of
    .v-enter-from  =>   .from_class {}
    .v-enter-active => .active_class {}
    .v-enter-to  =>  .to_class {}

-->




<!-- 8. Animating A Modal
======================================

-We can wrap our <transition></transition> Component  around our custom component   ; 

  <transition >
    <base-modal @close="hideDialog" v-if="dialogIsVisible">
        <p>This is a test dialog!</p>
        <button @click="hideDialog">Close it!</button>
    </base-modal>
  </transition>

Just as we can wrap around built in element;

    <transition name="para">
      <p v-if="isParaVisible"> Some text to toggle</p>
    </transition>

-And then we can add our own prefix to replace Vue built-in special utility prefixes

  <transition name="modal">
    <base-modal @close="hideDialog" v-if="dialogIsVisible">
        <p>This is a test dialog!</p>
        <button @click="hideDialog">Close it!</button>
    </base-modal>
  </transition>

-We can re-use the @keyframes modal {} INTHE BaseModal.vue  and use it here 

-And since we are using the animation we actually don't need .modal-enter-from{} & .modal-enter-to{} classes and we can add .modal-enter-active {} only

     .modal-enter-active {
      animation : modal 0.3s ease-out;
     }

-But if we do this, there is a problem because we don't see the animation applied

-However , the classes that Vue wants to add are added to the root element of our <base-modal></base-modal> custom component 

-This is the attribute fallthrough behavior that we saw a couple of videos ago

-However in  the BaseModal.vue we have 2 root elements : 
     1.  <div class="backdrop" @click="$emit('close')"></div>

    2.  <dialog open> <slot></slot> </dialog>

-If we comment out the first div ;  <div class="backdrop" @click="$emit('close')"></div> ; This WORKS

-But if we have both - Then this fails 

-And this is because <transition></transition> wants JUST ONE DIRECT CHILD ELEMENT ; 

-HOWEVER , the <base-modal></base-modal> is just used as a wrapper but inside the component we have 2 root elements 

     1.  <div class="backdrop" @click="$emit('close')"></div>

    2.  <dialog open> <slot></slot> </dialog>

  -So these 2 are the actual direct child elements inside our transition component and they are more than one and that is why this setup does not work

  -So how can we work around that :

      1. Split BaseModal into 2 components and manage the backdrop separately

      2. Remove the <transition></transition> wrapper around the custom component <base-modal></base-modal> and wrap just the <dialog></dialog> and try using it here
  
 -Then grab the @keyframes modal {} and .modal-enter-active{} from App.vue and transfer to BaseModal.vue

 -However, we need to change the logic how <base-modal></base-modal> opens

 -At the moment we are adding the <base-modal></base-modal> with v-if  directive

 -Therefore we will now open /close the modal differently  ; By adding :open="" as a prop and point to dialogIsVisible data property

 -As in forward the :open prop to <base-modal></base-modal> component 

 -And in the <base-modal></base-modal> component ; we can accept this prop here
 
  props: ['open'],
 
 
 -And simply use it in the dialog with v-if="open" to check if open is true or false to control whether the dialog is visible or not

  <transition name="modal">
     <dialog open v-if="open">
      <slot></slot>
    </dialog>
  </transition>

-And now we can add the v-if="open" to control the visibility of the backdrop

  <div v-if="open" class="backdrop" @click="$emit('close')"></div>

-And now we can actually work on a closing animation by adding .modal-leave-active{} as follows in the BaseModal.vue


.modal-leave-active {
  animation: modal 0.3s ease-out;
}

-However the animation is odd since we are using the same as for entering

-Therefore we can use the reverse property to reverse the animation of the rendering

.modal-leave-active {
  animation: modal 0.3s ease-out reverse;
}

:N/B - Tells the CSS to play the animation in reverse for .modal-leave-active{} scenario



-->



<template>
  <div class="container">
    <div class="block" :class="{ animate: animatedBlock }"></div>
    <button @click="animateBlock">Animate</button>
  </div>

  <div class="container">
    <transition>
      <p v-if="isParaVisible"> Some text to toggle</p>
    </transition>
    <button @click="toggleParagraph">{{ isParaVisible ? 'Hide' : 'Show' }} Paragraph</button>
  </div>


    <base-modal @close="hideDialog" :open="dialogIsVisible">
      <p>This is a test dialog!</p>
      <button @click="hideDialog">Close it!</button>
    </base-modal>

  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>

</template>  

<script>
export default {
  data() {
    return { 
      animatedBlock : false,
      dialogIsVisible: false,
      isParaVisible : false,
       };
  },


  methods: {

    animateBlock(){
      this.animatedBlock = true;
    },

    toggleParagraph(){
      this.isParaVisible = !this.isParaVisible;
    },

    showDialog() {
      this.dialogIsVisible = true;
    },

    hideDialog() {
      this.dialogIsVisible = false;
    },

  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.animate {
    animation: slide-scale 0.5s ease-out forwards;
}

/* <transition></transition>  special utility classes added by Vue when the elemnt is appearing/rendered to the DOM*/

    .v-enter-from {
      opacity: 0;
      transform: translateY(-30px);
    }

    .v-enter-active {
      transition : all 0.3s ease-out;
    }

    .v-enter-to {
      opacity : 1;
      transform: translateY(0);
    }

   /* <transition></transition>  special utility classes added by Vue when the elemnt is removed/detached from the DOM*/

     .v-leave-from {
       opacity: 1;
       transform: translateY(0);
        }

     .v-leave-active {
        transition : all 0.3s ease-in;
      
    }

      .v-leave-to {
       opacity: 0;
       transform: translateY(-30px);
     }


    
/* Animate Black Box */

@keyframes slide-scale {
  0%{
    transform : translateX(0) scale(1);
  }
 70% {
    transform: translateX(-100px) scale(1.1);
    }
  100% {
    transform: translateX(-150px) scale(1);
  }
}


</style>