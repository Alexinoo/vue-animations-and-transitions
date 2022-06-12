
 <!-- 3. Why is "just CSS " not enough ?
======================================

-Let's use the Show/Close Modal as an example

-We wanna animate when it appears and the same for removing it

-And that is the part which will be tricky with just CSS animations especially when you also wants to animate the disapperance

-Lets do this practically by animating our modal

          @keyframes modal{

            0% {   }

            100% {  }

          }

-If only to states are involved i.e. 0% and 100% ; You can also use from and to shorthands
   @keyframes modal{

            from {   }

            to {  }

          }
-So we want to move it down along the Yaxis and scale it by 0.9 

-Then back to its initial state/position this elemnt was with a scale of 1 to reach a scale of which this elemnt shd have

-i.e.

        @keyframes modal {
            from {
              opacity : 0;
              transform : translateY(-50px) scale(0.9)
            }
            to {
              opacity: 1;
              transform: translateY(0) scale(1)
            }
        }
-Now we can all our animation inside the .dialog{  } CSS rule

  dialog {
  animation: modal 0.3s ease-out;
}

-Then use forwards to tell CSS that the final state shd be kept

-BUT THE PROBLEM STARTS IF WE WANT TO ANIMATE THE REMOVAL OF THE ELEMNT - since if we close the modal , it disappears instantly

-The problem we have is that the elemnt is detached from the DOM instantly which means there is no way to animate it with CSS

-And that is exactly how vue is able to help us with that;

-Vue can help us animate the appearance and the disappearance of an element by delaying the disappearance until the animation is finished

-->

<template>

  <div v-if="open" class="backdrop" @click="$emit('close')"></div>

  <transition name="modal">
    <dialog open v-if="open">
      <slot></slot>
    </dialog>
  </transition>


</template>

<script>
export default {
  props: ['open'],
  emits: ['close'],
};
</script>

<style scoped>
.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 10;
  background-color: rgba(0, 0, 0, 0.75);
}

dialog {
  position: fixed;
  top: 30vh;
  width: 30rem;
  left: calc(50% - 15rem);
  margin: 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
  padding: 1rem;
  background-color: white;
  z-index: 100;
  border: none;
  /* animation: modal 0.3s ease-out forwards; - Commented to test out on  -- 8. Animating A Modal -- */
}

.modal-enter-active {
  animation: modal 0.3s ease-out;
}

.modal-leave-active {
  animation: modal 0.3s ease-in reverse;
}

@keyframes modal{

  from {
    opacity : 0;
    transform : translateY(-50px) scale(0.9);
  }

  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }

}

</style>