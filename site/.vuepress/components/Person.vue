<template>
  <div class="person full-bio">
    <a :href="person.website">
      <ImageHelper :image="person.image"
                   :imageAltText="person.imageAltText" />
    </a>
    <div class="person__info">
      <a :href="person.website" class="person__name"><h3>{{ person.name }}</h3></a>
      <div v-if="person.title" class="person__title">{{ person.title }}</div>
      <a v-if="person.affiliation" :href="person.affiliation.url" class="person__affiliation">{{ person.affiliation.name }} </a>
     <br/><br/>
     <div v-html="markdown(person.bio)" class="person__bio"></div>
    </div>
  </div>
</template>

<script>
import { markdown } from '../util'

export default {
  name: 'Person',
  props: {
    name: {
      type: String,
      required: false
    }
  },

  computed: {
    person (name) {
      var people = this.$site.pages
        .filter(x => x.path.startsWith('/people/') && x.frontmatter.name == this.name)
      return people[0] && people[0].frontmatter
    }
  },

  methods: { markdown }
}
</script>

<style lang="scss">
@import '../assets/stylesheets/variables.scss';

// Change these!
$person-width: 125px;
$person-stroke: 3px;

// Don't change these!
$person-height: $person-width * 1.15;
$person-outline-width: $person-width + ($person-stroke * 2);
$person-outline-height: $person-height + ($person-stroke * 2);

.person {
  align-items: center;
  border: 3px solid $color-green;
  display: flex;
  flex-direction: row;
  height: max-content;
  margin-bottom: $space-medium;
  padding: $space-base;
  position: relative;

  @include mobile {
    flex-direction: row;
    margin-bottom: 5%;
    margin-right: 5%;
    width: 45%;
  }

  &.full-bio {
    flex-direction: column;

    figure { margin-right: 0; }
  }

  figure {
    background: $color-green;
    clip-path: polygon(49% 0, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
    height: $person-outline-height;
    margin-bottom: $space-small;
    margin-right: $space-medium;
    position: relative;
    width: $person-outline-width;

    img {
      clip-path: polygon(49% 0, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
      height: $person-height;
      left: $person-stroke;
      object-fit: cover;
      position: absolute;
      top: $person-stroke;
      width: $person-width;
    }
  }

  &__title {
    font-weight: bold;
    text-transform: uppercase;
  }

  &__affiliation {
    color: $color-green;
    font-weight: bold;
    text-transform: uppercase;
  }

  &__bio {
    display: none;

    .full-bio & {
      display: block;
    }
  }
}
</style>
