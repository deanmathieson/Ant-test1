<template>
  <div
    class="modal invisible max-w-max w-full max-w-prose p-4"
    :style="{
      backgroundColor: colours[0].rgba,
      border: '3px solid ' + colours[2].hex,
    }"
  >
    <div>
      <p class="text-3xl">
        <span>📧</span><span> Contact me</span>
        <span class="inline-block reversed">📧 </span>
      </p>
    </div>
    <form
      name="contactus"
      method="post"
      netlify
      netlify-honeypot="bot-field"
      class="w-full flex flex-col space-y-4"
    >
      <input type="hidden" name="form-name" value="contactus" />
      <div class="opacity-0">
        <label class="text-lg" for="name">Name:</label>
        <input class="text-black" type="text" name="name" required />
      </div>
      <div class="opacity-0">
        <label class="text-lg" for="email">Email:</label>
        <input class="text-black" type="email" name="email" required />
      </div>
      <div class="opacity-0">
        <label class="text-lg" for="message">Message:</label>
        <textarea class="text-black" name="message" required></textarea>
      </div>
      <button
        :style="{
          backgroundColor: colours[1].hex,
          border: '3px solid ' + colours[2].hex,
          color: '#000',
        }"
        ref="sendBtn"
        type="submit"
        value="Send message"
        @mouseenter="sendEnter"
        @mouseleave="sendLeave"
      >
        Send
      </button>
    </form>
  </div>
</template>

<script>
export default {
  data() {},
  methods: {
    sendEnter() {
      gsap.to(this.$refs.sendBtn, {
        backgroundColor: this.colours[2].hex,
        borderColor: this.colours[1].hex,
        duration: 0.8,
      });
    },
    sendLeave() {
      gsap.to(this.$refs.sendBtn, {
        backgroundColor: this.colours[1].hex,
        borderColor: this.colours[2].hex,
        duration: 0.8,
      });
    },
    init() {
      let modals = this.$refs.contactModal;
      for (let i = 0; i < this.$refs.buttons.length; i++) {
        let buttonLocation = this.$refs.buttons[i].getBoundingClientRect();
        this.tl[i] = gsap.timeline();
        this.tl[i].fromTo(
          modals[i],
          {
            top: buttonLocation.top,
            left: buttonLocation.left,
            zIndex: this.zIndex,
          },
          {
            visibility: "visible",
            top: "50%",
            left: "50%",
          }
        );
        modals[i].children.forEach((item) => {
          item.children.forEach((child) => {
            this.tl[i].fromTo(
              child,
              {
                x: 50,
                y: 5,
                opacity: 0,
              },
              {
                y: 0,
                x: 0,
                opacity: 1,
                onComplete: () => {
                  this.tl[i].animating = false;
                },
              }
            );
            if (child.children.length > 0) {
              child.children.forEach((child) => {
                this.tl[i].fromTo(
                  child,
                  {
                    x: 50,
                    y: 5,
                    opacity: 0,
                  },
                  {
                    y: 0,
                    x: 0,
                    opacity: 1,
                    onComplete: () => {
                      this.tl[i].animating = false;
                    },
                  }
                );
              });
            }
          });
        });
        this.tl[i].modal = modals[i];
        this.tl[i].duration(2);
        this.tl[i].pause();
      }
    },
  },
  props: {
    colours: {
      type: Array,
      default: [],
    },
  },
};
</script>

<style lang="scss" scoped>
</style>