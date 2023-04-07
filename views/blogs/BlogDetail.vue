<template>
    <div class="container is-widescreen">
        <section v-if="error" class="section">
            <div class="container is-widescreen">
                <div class="notification is-danger">
                    {{ error.code + ': ' + error.sqlMessage }}
                </div>
            </div>
        </section>
        <!-- <% } else { %> -->
        <section v-else class="hero">
            <div class="hero-body">
                <p class="title">
                    {{ blog.title }}
                </p>
            </div>
        </section>
        <section class="section" id="app">
            <div class="content">
                <div class="card has-background-light">
                    <div class="card-image pt-5">
                        <div class="columns">
                            <!-- <% for (let image of images) { %> -->
                            <div v-for="image in images" :key="image" class="column">
                                <figure class="image">
                                    <img :src="image.file_path ? image.file_path : 'https://bulma.io/images/placeholders/480x480.png'"
                                        alt="Placeholder image">
                                </figure>
                            </div>
                            <!-- <% } %> -->
                        </div>

                    </div>
                    <div class="card-content">
                        <div class="content">
                            {{ blog.content }}
                        </div>
                        <div class="container">
                            <p class="subtitle">Comments</p>
                            <!-- <% for (comment of comments) { %> -->
                            <div v-for="comment in comments" :key="comment" class="box">
                                <article class="media">
                                    <div class="media-left">
                                        <figure class="image is-64x64">
                                            <img :src="comment.file_path ? comment.file_path : 'https://bulma.io/images/placeholders/128x128.png'"
                                                alt="Image">
                                        </figure>
                                    </div>
                                    <div class="media-content">
                                        <div class="content">
                                            <p>
                                                {{ comment.comment }}
                                            </p>
                                            <p class="is-size-7">
                                                {{ comment.comment_date }}
                                            </p>
                                        </div>
                                        <nav class="level is-mobile">
                                            <div class="level-left">
                                                <a class="level-item" aria-label="like">
                                                    <span class="icon is-small">
                                                        <i class="fas fa-heart" aria-hidden="true"></i>
                                                    </span>
                                                </a>
                                            </div>
                                        </nav>
                                    </div>
                                </article>
                            </div>
                            <!-- <% } %> -->
                            <!-- action="<%= `/${blog.id}/comments/` %>"  -->
                            <div class="columns box">
                                <div class="column is-7">
                                    <input class="input" type="text" name="comment" placeholder="Comment here..." value="">
                                </div>
                                <div class="column is-3">
                                    <div class="file">
                                        <label class="file-label">
                                            <input class="file-input" type="file" name="comment_image">
                                            <span class="file-cta">
                                                <span class="file-icon">
                                                    <i class="fas fa-upload"></i>
                                                </span>
                                                <span class="file-label">
                                                    Choose an image…
                                                </span>
                                            </span>
                                        </label>
                                    </div>
                                </div>
                                <div class="column is-2">
                                    <input class="button is-primary" type="submit" @click="submit()" value="Submit">
                                </div>
                            </div>
                        </div>
                    </div>
                    <footer class="card-footer">
                        <a class="card-footer-item" href="/">To Home Page</a>
                    </footer>
                </div>
            </div>
        </section>
        <!-- <% } %> -->
    </div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            blogs: null,
            file: null,
        }
    },
    created() {
        axios.get(`http://localhost:3000/blogs/${this.$route.params.id}`)
            .then((response) => {
                //   this.blogs = response.data;
                console.log(response.data)
                this.blogs = response.data.blog
                //   this.content = response.data.content

            })
            .catch((err) => {
                console.log(err);
            });
    },
    methods: {
        handleFileUpload() {
            this.file = this.$refs.file.files[0];
        },
        submit() {
            var formData = new FormData();
            formData.append("blog_image", this.file);
            formData.append("title", this.title)
            formData.append("content", this.content)
            axios.post('http://localhost:3000/blogs/:id', formData, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            }).then(response => {
                this.$router.push({ path: '/blogs/:id' }) // Success! -> redirect to home page
                return response
            })
                .catch(error => {
                    console.log(error.message);
                });
        }
    },
}
</script>