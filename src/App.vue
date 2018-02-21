<template>
  <div id="projects">
    <h2>Projects</h2>
    <div class="projects2">
      <div 
        v-for="project, index in projects" 
        class="proj " 
        :style="{backgroundImage: 'url('+project.image_link+')'}"
        @click="expandPreview($event, index)">
        <span class="name">{{ project.name }}</span>
        <span class="links">
          <a :href="project.github_repo">GitHub Repo</a>
          <a :href="project.demo_url">Demo</a>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "projects",
  data () {
    return {
      project_names: [
        "Movie Watch List",
        "Bill Tracker",
      ],
      projects: [
        {
          name: "Movie Watch List",
          github_repo: "https://github.com/arpiper/movie-list",
          demo_url: "https://arpiper.github.io/movie-list",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_movielist2.jpg",
          description: "Movie list",
        },
        {
          name: "Roommate Bill Tracker",
          github_repo: "https://github.com/arpiper/ngbills",
          demo_url: "https://arpiper.github.io/ngbills",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_billtracker.jpg",
          description: "Bill tracker built using Angular",
        },
        {
          name: "Django Blog App",
          github_repo: "https://github.com/arpiper/django-pipes-blog",
          demo_url: "",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_blog.jpg",
          description: "blog",
        },
        {
          name: "vue.js tetris",
          github_repo: "https://github.com/arpiper/tetris",
          demo_url: "https://arpiper.github.io/tetris",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_tetris.jpg",
          description: "tetris",
        },
        {
          name: "chaos game",
          github_repo: "https://github.com/arpiper/chaosgame",
          demo_url: "https://arpiper.github.io/chaosgame",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_chaos.jpg",
          description: "numberphile inspired chaos game.",
        },
        {
          name: "NASA NEO",
          github_repo: "https://github.com/arpiper/nasa_neo",
          demo_url: "https://arpiper.github.io/projects/nasa_neo",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/port_nasaneo.jpg",
          description: "NASA near earth objects orbits animated using d3.js and vue.js",
        },
      ],
      current_project: 1,
    }
  },
  methods: {
    pickProject: function (index) {
      this.current_project = index
    },
    expandPreview: function (evt, index) {
      evt.stopPropagation;
      evt.preventDefault();
      evt.target.className += " previewed"
      let e = document.createElement("iframe")
      e.src = this.projects[index].demo_url
      evt.target.appendChild(e)
      let c = document.createElement("span")
      c.className = "cross-x"
      c.onclick = this.closePreview(evt.target)
      evt.target.appendChild(c)
    },
    closePreview: function (el) {
      console.log("close", el)
    },
  },
}
</script>

<style>
#projects {
}
/*
 * grid layout.
 */
.projects2 {
  display: grid;
  width: 100%;
  grid-template-rows: repeat(3, 250px);
  grid-template-columns: repeat(auto-fill, minmax(33%, 1fr));
}
.proj {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
  justify-content: flex-start;
  background-size: cover;
  align-items: flex-start;
}
.proj span.name,
.proj span.links {
  background-color: rgba(76, 175, 80, 0.5);
  background-color: var(--color-p-dark);
  opacity: 0.8;
  padding: 10px;
  margin: 10px;
}
.proj span.links a {
  color: var(--color-s-dark);
  display: block;
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
  z-index: 101;
  width: 100%;
  height: 100%;
}
.cross-x {
  position: absolute;
  top: 10px;
  right: 20px;
}
</style>
