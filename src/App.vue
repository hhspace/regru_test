<template lang="pug">
  .app
    .app__title
      h1.title Новостная лента REG.RU
    .app__search
      form
        input(placeholder='Поиск...')
    .app__list
      .app__list-item.item(v-for='post in news')
        .item__title
          h3 {{ post.title[0] }}
        .item__text(v-html='post.description[0]')
</template>

<style lang="scss">
  @import url('https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.0/normalize.css');
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    font: 18px/26px Arial, Helvetica, sans-serif;
    color: #333;
    background-color: #eee;
    backface-visibility: hidden;
    
    a {
      transition: .3s ease-out;

      &.b-button {
        border: 2px solid;
        font-size: 1em;
        text-decoration: none;
        display: inline-block;
      }
      &.b-button_color_primary {
        background-color: #fff;
        color: blue;
        border-color: blue;

        &:hover,
        &:focus {
          color: #fff;
          background-color: blue;
          text-decoration: none;
        }
      }
      &.b-button_size_big {
        letter-spacing: .05em;
        padding: 1em 2em;
        text-transform: uppercase;
        font-weight: 600;

        @media (max-width: 1200px) {
          letter-spacing: 0;
          padding: .75em 1.5em;
        }
        @media (max-width: 479px) {
          letter-spacing: 0;
          display: block;
          text-align: center;
          padding: .5em;
        }
      }
    }
    iframe {
      width: 100% !important;
    }
    .app {
      background-color: #fff;
      width: 100%;
      max-width: 988px;
      margin: 0 auto;
      padding: 1em;

      &__title {
        h1 {
          line-height: 1.2;
        }
      }

      &__search {
        padding: 1em;
        background-color: #eee;

        input {
          font-size: 1em;
          padding: .5em;
          width: 100%;
          border: 1px solid transparent;
          transition: .25s ease-out;
          outline: none;

          &:focus {
            border-color: blue;
            box-shadow: inset 0 2px 4px #eee;
          }
        }
      }
      &__list {
        .app__list-item {
          &.item {
            display: block;
            position: relative;
            border-bottom: .5em solid #eee;
            padding-bottom: 4em;
            
            .item__title {
              margin-top: 4em;
              line-height: 1.2;
            }

            &:first-child {
              .item__title {
                margin-top: 2em;
              }
            }
            .item__text {
              display: block;
              position: relative;
              width: 100%;
              margin-top: 1em;
              padding-right: 35%;
              line-height: 1.35;
              
              @media (max-width: 1024px) {
                padding-right: 0;
              }

              img {
                width: auto;
                max-width: 100%;
                margin: 0 auto;
                display: block;
                margin:  0 auto 1em;

                &.b-news-article__right {
                  display: block;
                  position: absolute;
                  right: 0;
                  top: 0;
                  margin: 0 0 0 1em;
                  width: 30%;

                  @media (max-width: 1024px) {
                    display: block;
                    position: relative;
                    width: auto;
                    float: none;
                    margin: 0 auto;
                  } 
                  @media (max-width: 479px) {
                    display: none;
                  } 
                }

                @media (max-width: 479px) {
                  display: none;
                } 
              }
            }
          }
        }
      }
    }
  }
</style>

<script>
  import axios from 'axios'
  import { parser, parseString } from 'xml2js';


  export default {
    data() {
      return {
        news: []
      }
    },
    mounted() {
      axios.get('https://cors.now.sh/https://www.reg.ru/company/news/rss', { 
        headers: { 'Accept': 'application/xml' }
      })

      .then(response => {
        var self = this; 
        parseString(response.data, function (err, result) {
          self.news = result.rss.channel[0].item;
        });        
      })
      .catch((error) => {
        console.log({ error });
      })
    }
  };
</script>

