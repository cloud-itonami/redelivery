(ns redelivery-frontend.app
  "etzhayyim-wasm-redelivery-rd3l1vry appview frontend shell.

  Faithful ClojureScript replacement of the former Svelte scaffold
  (`svelte/src/App.svelte` — a one-line placeholder: an <h1> with the repo
  name and a single sentence of body copy, centered on the page). This does
  not add functionality the Svelte scaffold did not have; it is the same
  static shell, now built on this workspace's standard stack (reagent +
  re-frame + jp-go-dds) instead of inventing behavior that was never
  actually implemented in svelte/src/App.svelte."
  (:require [reagent.dom :as rdom]
            [re-frame.core :as rf]
            [jp-go-dds.core :as dds]))

;; -- app-db ------------------------------------------------------------

(def default-db
  {:name "etzhayyim-wasm-redelivery-rd3l1vry"
   :tagline "ClojureScript entry scaffold after Svelte migration."})

;; -- events --------------------------------------------------------------

(rf/reg-event-db
 :init-db
 (fn [_ _] default-db))

;; -- subs ------------------------------------------------------------------

(rf/reg-sub
 :app/name
 (fn [db _] (:name db)))

(rf/reg-sub
 :app/tagline
 (fn [db _] (:tagline db)))

;; -- view --------------------------------------------------------------

(defn view []
  (let [name @(rf/subscribe [:app/name])
        tagline @(rf/subscribe [:app/tagline])]
    ;; dds-ext-hero (vendored in jp-go-dds's ext-css, see core.cljc) centers
    ;; and pads its content the same way the Svelte scaffold's hand-written
    ;; `main { min-height:100vh; display:grid; place-content:center; }` did.
    [dds/container
     [:div {:class "dds-ext-hero"}
      [dds/heading 1 name]
      [:p tagline]]]))

;; -- mount ---------------------------------------------------------------

(defn main []
  (rf/dispatch-sync [:init-db])
  (rdom/render [view] (.getElementById js/document "app")))
