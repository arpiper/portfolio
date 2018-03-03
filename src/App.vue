<template>
  <div id="portfolio">
    <div class="portfolio-block">
      <h2>Technologies</h2>
      <div class="technologies">
        <span v-for="tech in technologies" class="tech">
          <span>{{ tech.name }}</span>
        </span>
      </div>
    </div>

    <div class='portfolio-block'>
      <h2>Projects</h2>
      <div class="projects-container">
        <div 
          class="project-container"
          v-for="project, index in projects">
          <div class="project"
          :style="{backgroundImage: 'url('+project.image_link+')'}">
            <span class="name">{{ project.name }}</span>
            <span class="links">
              <a :href="project.github_repo">GitHub Repo</a>
              <a v-if="project.demo_url.length > 0" :href="project.demo_url">Demo</a>
            </span>
            <span class="text">
              {{ project.description }}
            </span>
          </div>
        </div>
      </div>
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
      projects2: [
        {
          name: "Movie Watch List",
          github_repo: "https://github.com/arpiper/movie-list",
          demo_url: "https://arpiper.github.io/movie-list",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_movielist2.jpg",
          description: "Movie wacth list built with Vue.js",
        },
        {
          name: "Roommate Bill Tracker",
          github_repo: "https://github.com/arpiper/ngbills",
          demo_url: "https://arpiper.github.io/projects/ngbills",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_billtracker2.jpg",
          description: "Bill tracker built using Angular",
        },
        {
          name: "Django Blog App",
          github_repo: "https://github.com/arpiper/django-pipes-blog",
          demo_url: "",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_blog.jpg",
          description: "blog app built for Django.",
        },
        {
          name: "vue.js tetris",
          github_repo: "https://github.com/arpiper/tetris",
          demo_url: "https://arpiper.github.io/tetris",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_tetris.jpg",
          description: "Tetris built with Vue.js",
        },
        {
          name: "chaos game",
          github_repo: "https://github.com/arpiper/chaosgame",
          demo_url: "https://arpiper.github.io/chaosgame",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_chaos.jpg",
          description: "numberphile inspired 'chaos game' showing fun geometric properties.",
        },
        {
          name: "NASA NEO",
          github_repo: "https://github.com/arpiper/nasa_neo",
          demo_url: "https://arpiper.github.io/projects/nasa_neo",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_nasaneo.jpg",
          description: "NASA near earth objects orbits animated using D3.js and Vue.js",
        },
      ],
      current_project: 1,
      technologies: [
        {name: "Python", logo: ""},
        {name: "Django", logo: ""},
        {name: "PHP", logo: ""},
        {name: "JavaScript", logo: ""},
        {name: "Angular", logo: ""},
        {name: "Vue.js", logo: ""},
        {name: "D3.js", logo: ""},
        {name: "Svelte", logo: ""},
        {name: "HTML", logo: ""},
        {name: "CSS", logo: ""},
      ],
      projects: [],
    }
  },
  methods: {
    getData: function (url, data_key) {
      fetch(url)
        .then( (response) => {
          return response.json()
        })
        .then(res => {
          //this.projects = res.projects
          this.$set(this._data, data_key, res)
        })
    },
    pickProject: function (index) {
      this.current_project = index
    },
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
    closePreview: function (evt) {
      evt.stopPropagation
      evt.preventDefault()
      if (evt.target.className === "cross-x") {
        document.querySelector(".previewed").className = "hidden"
      }
    },
  },
  mounted () {
    this.getData("https://arpiper.com/api/projects", "projects")
    this.getData("https://arpiper.com/api/tech", "tech")
  }
}
</script>

<style>
#projects {
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
@media screen and (max-width: 800px) {
  .projects-container {
    grid-template-columns: 100%;
  }
}
</style>
