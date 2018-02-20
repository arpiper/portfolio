<template>
  <div id="projects">
    <div class="projects2">
      <div v-for="project in projects" class="proj" :style="{backgroundImage: 'url('+project.image_link+')'}">
        <span class="name">{{ project.name }}</span>
        <span class="links">
          <a :href="project.github_repo">GitHub Repo</a>
          <a :href="project.demo_url">Demo</a>
        </span>
      </div>
    </div>

    <h2>Projects</h2>
    <div class="projects">
      <div class="project-tabs">
        <span v-for="project, index in projects" @click="pick_project(index)" :class="{ front: index==current_project}">
          {{ project.name }}
        </span>
      </div>
      <div v-for="project, index in projects" class="project" :class="{ front: index==current_project }">
        <div class="info">
          <h3>{{ project.name }}</h3>
          <span class="github-repo">
            <a :href="project.github_repo">Github</a>
          </span>
          <span class="demo-url">
            <a :href="project.demo_url">Demo</a>
          </span>
        </div>
        <div class="description">
          <p>{{ project.description }}</p>
        </div>
        <div class="live-demo">
          <iframe :src="project.demo_url"></iframe>
        </div>
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
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/2018/01/09/600x600/movie-list.jpg",
          description: "Movie list",
        },
        {
          name: "Roommate Bill Tracker",
          github_repo: "https://github.com/arpiper/ngbills",
          demo_url: "https://arpiper.github.io/ngbills",
          image_link: "https://storage.googleapis.com/pipes-stor/media/featured_images/2018/01/10/600x600/3279c000-d12a-48db-946b-f5331d91660b.jpg",
          description: "Bill tracker built using Angular"
        },
      ],
      current_project: 1,
    }
  },
  methods: {
    pick_project: function (index) {
      this.current_project = index
    },
  },
}
</script>

<style>
#projects {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #212121;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #4caf50;
}
.projects {
  display: flex;
  flex-wrap: wrap;
  position: relative;
  height: 700px;
  width: 100%;
  overflow: hidden;
}
.project-tabs {
  display: flex;
  height: 30px;
  justify-content: flex-start;
  align-items: center;
}
.project-tabs span {
  height: 100%;
  padding: 0 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  border-bottom: 2px solid #dbdbdb;
  position: relative;
}
.projects span:before {
  height: 0;
  content: "";
  width: 85%;
  border-radius: 3px;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
  border-bottom: 30px solid #757575;
  position: absolute;
  z-index: -10;
  top: 0;
  left: 0;
}
.project-tabs span.front {
  border-bottom: none;
  z-index: 5;
}
.project {
  display: flex;
  width: 100%;
  height: calc(100% - 30px);
  color: #e0e0e0;
  flex-direction: column;
  background-color: #757575;
  padding: 15px;
  position: absolute;
  top: 30px;
  left: 0;
}
.project div {
}
.project .info h3 {
  margin-bottom: 0;
}
.project .live-demo {
  flex: 1;
}
.project.front {
  z-index: 100;
}
.live-demo iframe {
  width: 80%;
  height: 100%;
}

/*
 * differnet layout.
 */
.projects2 {
  display: grid;
  width: 85%;
  grid-template-rows: repeat(3, 250px);
  grid-template-columns: repeat(auto-fill, minmax(33%, 1fr));
}
.proj {
  display: flex;
  width: 100%;
  height: 100%;
  padding: 20px;
  flex-direction: column;
  justify-content: flex-start;
}
</style>
