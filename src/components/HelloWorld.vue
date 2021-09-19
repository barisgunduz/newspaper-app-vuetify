<template>
    <v-app id="inspire">
        <v-app-bar app color="white" flat>
            <v-container class="py-0 fill-height">
                <v-avatar
                    class="mr-10"
                    color="grey darken-1"
                    size="32"
                ></v-avatar>

                <v-btn v-for="link in links" :key="link" text>
                    {{ link }}
                </v-btn>

                <v-spacer></v-spacer>

                <v-responsive max-width="260">
                    <v-text-field
                        dense
                        flat
                        hide-details
                        rounded
                        solo-inverted
                    ></v-text-field>
                </v-responsive>
            </v-container>
        </v-app-bar>

        <v-main class="grey lighten-3">
            <v-container>
                <v-row>
                    <v-col>
                        <v-sheet min-height="70vh" rounded="lg">
                            <!--  -->
                            <div class="container">
                                <div class="row">
                                    <div
                                        class="col col-4"
                                        v-for="latestNew in latestNews"
                                        v-bind:key="latestNew.id"
                                    >
                                        <v-card class="mx-auto" max-width="344">
                                            <v-img
                                                :src="latestNew.urlToImage"
                                                height="200px"
                                            ></v-img>

                                            <v-card-title>
                                                {{ latestNew.title }}
                                            </v-card-title>

                                            <v-card-subtitle>
                                                {{ latestNew.description }}
                                            </v-card-subtitle>

                                            <v-card-actions>
                                                <v-btn
                                                    color="orange lighten-2"
                                                    text
                                                    :href="latestNew.url"
                                                    target="_blank"
                                                >
                                                    {{ latestNew.author }}
                                                </v-btn>
                                            </v-card-actions>
                                        </v-card>
                                    </div>
                                </div>
                            </div>
                        </v-sheet>
                    </v-col>
                </v-row>
            </v-container>
        </v-main>
    </v-app>
</template>

<script>
import axios from "axios";
export default {
    data: () => ({
        show: false,
        links: ["Anasayfa", "Dünyadan", "Spor", "Updates"],
        latestNews: null,
        loading: true,
        errored: false,
    }),
    created: function() {
        axios
            .get(
                "https://newsapi.org/v2/top-headlines?country=tr&apiKey=c524fd11899e40358afec60bd7d0f87b"
            )
            .then((res) => {
                let fullArticles = res.data.articles.filter(
                    (article) =>
                        article.author &&
                        article.url &&
                        article.description &&
                        article.title
                );
                this.latestNews = fullArticles;
                console.log(fullArticles);
            })
            .catch((error) => {
                console.log(error);
                this.errored = true;
            })
            .finally(() => (this.loading = false));
    },
};
</script>
