<template>
  <div id="portfolio">
    <transition appear>
      <div class="portfolio-block" v-if="about" key='about' data-key='1'>
        <h2>About Me</h2>
        <p v-for="text in about.about_text">
          {{ text }}
        </p>
      </div>
    </transition>
    <transition appear>
      <div class="portfolio-block" key='tech' data-key='2' v-if="technologies">
        <h2>Technologies</h2>
        <div class="technologies">
          <span v-for="tech in technologies" class="tech">
            <span>{{ tech }}</span>
          </span>
        </div>
      </div>
    </transition>
    <div class='portfolio-block' key='projects' data-key='3' >
      <transition appear>
        <h2>Projects</h2>
      </transition>
      <transition-group name="projects" 
        tag="div" 
        @before-enter="beforeEnter"
        @enter="enter"
        class="projects-container">
        <div 
          class="project-container"
          v-for="project, index in projects"
          :key="index"
          :data-key="index">
          <div class="project"
          :style="{backgroundImage: 'url('+project.image_link+')'}">
            <span class="name">{{ project.name }}</span>
            <span class="links">
              <a :href="project.github_repo">GitHub Repo</a>
              <a v-if="project.demo_url.length > 0" :href="project.demo_url" target="_blank">Demo</a>
            </span>
            <span class="text">
              {{ project.description }}
            </span>
          </div>
        </div>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  name: "portfolio",
  data () {
    return {
      project_names: [
        "Movie Watch List",
        "Bill Tracker",
      ],
      current_project: 1,
      technologies: undefined,
      projects: undefined,
      about: undefined,
    }
  },
  methods: {
    getData: function (url) {
      fetch(url)
        .then( (response) => {
          return response.json()
        })
        .then(res => {
          this.projects = res.projects
          this.projects.sort((a,b) => {
            if (parseInt(a.order.split(":")[1]) > parseInt(b.order.split(":")[1])) {
              return 1
            }
            if (parseInt(a.order.split(":")[1]) < parseInt(b.order.split(":")[1])) {
              return -1 
            }         
            return 0
          })
          this.about = res.about
          this.technologies = res.tech
        })
    },
    /* transition functions */
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      let delay = el.dataset.key * 200
      console.log(delay, el)
      setTimeout(() => {
        el.style.opacity = 1
      }, delay)
    },
    /* unused */
    pickProject: function (index) {
      this.current_project = index
    },
    /* unused */
    expandPreview: function (evt, index) {
      evt.stopPropagation
      evt.preventDefault()
      if (evt.target.className === "project") {
        let el = evt.target.querySelector(".hidden")
        if (!el.firstChild.src) {
          el.firstChild.src = this.projects[index].demo_url
        }
        el.className = "previewed"
      }
    },
    /* unused */
    closePreview: function (evt) {
      evt.stopPropagation
      evt.preventDefault()
      if (evt.target.className === "cross-x") {
        document.querySelector(".previewed").className = "hidden"
      }
    },
  },
  created () {
    this.getData("https://arpiper.com/api/portfolio")
  },
}
</script>

<style>
#projects {
  transition: opacity 1s;
}
/*
 * grid layout.
 */
.projects-container {
  display: grid;
  width: 100%;
  position: relative;
  grid-template-rows: repeat(auto-fill, 250px);
  grid-template-columns: repeat(auto-fill, minmax(33%, 1fr));
}
.project-container { 
  position: relative;
  width: 100%;
  height: 100%;
  min-height: 250px;
  transition: opacity 1s;
}
.project {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
  justify-content: flex-start;
  background-size: cover;
  align-items: flex-start;
  transition: transform .33s;
}
.project:hover {
  position: absolute;
  transform: scale(1.1);
  box-shadow: 2px 2px 5px 0 black;
  z-index: 101;
  transition: transform .33s, opacity .33s;
}
.project span.name,
.project span.links a {
  background-color: var(--color-p-dark);
  opacity: 0.8;
  padding: 10px;
  margin: 10px;
}
.project span.links a {
  color: var(--color-s-dark);
  font-size: 12px;
  padding: 5px;
  display: inline-block;
}
.project span.text {
  opacity: 0;
  margin-top: auto;
  margin-bottom: 10px;
  padding: 10px;
  align-self: center;
  transition: opacity .33s;
}
.project:hover span.text {
  opacity: 0.85;
  background-color: var(--color-s-light);
  transition: opacity .33s;
}
.tint {
  position: relative;
}
.tint:before {
  position: absolute;
  content: '';
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.1);
}
.tint div, 
.tint span {
  z-index: 1;
}
.previewed {
  position: absolute;
  top: 10%;
  left: 10%;
  width: 80%;
  height: 80%;
  z-index: 100;
}
.previewed iframe {
  position: absolute;
  background-color: #ffffff;
  width: 100%;
  height: 100%;
}
.cross-container {
  top: 10px;
  right: 20px;
}
.hidden {
  display:none;
}
.technologies {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}
.tech {
  padding: 10px;
  margin: 10px;
  background-color: var(--color-primary);
  color: var(--color-font-dark);
}
.v-enter-active,
.appear-enter-before {
  transition: opacity .5s;
}
.v-enter,
.appear-enter {
  opacity: 0;
}
@media screen and (max-width: 800px) {
  .projects-container {
    grid-template-columns: 100%;
  }
}
</style>
