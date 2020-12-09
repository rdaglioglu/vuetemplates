import Vue from "vue";
import Cleave from "cleave.js";

function getInput(el) {
  if (el.tagName.toLocaleUpperCase() !== "INPUT") {
    const els = el.getElementsByTagName("input");
    if (els.length !== 1) {
      throw new Error(`cleave requires 1 input, found ${els.length}`);
    } else {
      el = els[0];
    }
  }
  return el;
}

let inputListener = null;

Vue.directive("cleave", {
  bind: (el, binding, vnode) => {
    const elm = getInput(el);
    elm.cleave = new Cleave(elm, binding.value || {});

    inputListener = function(e) {
      if (e.target.cleave) {
        const value = e.target.cleave.properties.result;

        const eventName = "input";
        if (vnode.componentInstance) {
          vnode.componentInstance.$emit(eventName, value);
        } else {
          vnode.elm.dispatchEvent(new CustomEvent(eventName, value));
        }
      }
    };
    el.addEventListener("input", inputListener);
  },
  unbind: (el) => {
    const elm = getInput(el);
    if (elm.cleave) {
      elm.cleave.destroy();
    }
    if (inputListener) {
      el.removeEventListener("input", inputListener);
    }
  }
});
