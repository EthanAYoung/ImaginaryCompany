<template>
    <div class="weather-component">

        <div class="display-section">
            <div class="text-column">
                <h2>Welcome to The Imaginary Company - Your Gateway to Software Innovation!</h2>

                <p>At The Imaginary Company, we are pioneers in the dynamic world of software solutions. Established in 2023, our journey began with a vision to redefine the way you experience technology. We specialize in curating and delivering a diverse range of software versions, ensuring that you stay at the forefront of innovation.</p>

                <h3 class="new-h3-block">Our Mission</h3>
                <p>Our mission is simple yet profound - to empower individuals and businesses with cutting-edge software that enhances productivity, creativity, and overall digital experiences. We believe in making the imaginary real, turning your software dreams into tangible solutions.</p>

                <h3 class="new-h3-block">Why Choose Us?</h3>
                <p> Unparalleled Selection. Dive into a vast selection of software versions, each carefully curated to meet the evolving needs of our diverse clientele. From industry-standard applications to avant-garde innovations, we have it all.</p>

                <h4 class="new-h4-block">Innovation Hub</h4>
                <p>Embrace the future with our software innovations. Our team of imaginative developers constantly pushes boundaries, introducing you to features and functionalities that redefine the software landscape.</p>

                <h4 class="new-h4-block">Customer-Centric Approach</h4>
                <p>Your satisfaction is our priority. Our dedicated support team is here to assist you at every step, ensuring a seamless experience from exploration to implementation.</p>

                <h4 class="new-h4-block">Imaginary Guarantee</h4>
                <p>Rest easy with our Imaginary Guarantee - a commitment to quality, security, and continuous improvement. We stand by the excellence of our software versions, setting the standard for reliability.</p>

                <h4 class="new-h4-block">Join Us in Shaping the Future</h4>
                <p>As The Imaginary Company continues to evolve, we invite you to be part of our journey. Explore, experience, and envision the possibilities with our innovative software solutions. Together, let's shape a future where the imaginary becomes reality. Discover the power of imagination with The Imaginary Company - Your Partner in Software Excellence.</p>
            </div>
            <div class="results-column">
                <div class="results-content">
                    <h3>View Our Products Below</h3>
                    <p>Version Number:</p>
                    <div class="input-row">
                        <input type="text" v-model="version" class="search-box" />
                        <input type="button" value="Search" @click="searchResults" class="search-button" />
                    </div>
                    <div class="results-wrapper">
                        <div v-if="loading" class="loading">
                            Loading Data
                        </div>


                        <div v-if="!loading && filteredResults.length != 0" class="content">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Name</th>
                                        <th>Version</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="software in filteredResults" :key="software.version">
                                        <td>{{ software.name }}</td>
                                        <td>{{ software.version }}</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <div v-if="!loading && filteredResults.length == 0" class="loading">
                            There are no available versions
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="js">
    import { defineComponent } from 'vue';

    export default defineComponent({
        data() {
            return {
                loading: true,
                post: null,
                filteredResults: null,
                version: null
            };
        },
        created() {
            // fetch the data when the view is created and the data is
            // already being observed
            this.fetchData();
        },
        watch: {
            // call again the method if the route changes
            '$route': 'fetchData'
        },
        methods: {
            fetchData() {
                this.post = null;
                this.loading = true;

                fetch('weatherforecast')
                    .then(r => r.json())
                    .then(json => {
                        this.post = json;
                        this.post = this.post.sort(this.compareSoftware);
                        this.loading = false;
                        this.filteredResults = this.post;
                        return;
                    });
            },
            searchResults() {
                this.loading = true;
                if (!this.version) {
                    this.fetchData();
                    return;
                }
                this.filteredResults = this.post.filter((software) => this.compareVersions(this.version, software.version) <= 0);
                console.log(this.filteredResults);
                this.loading = false;
            },
            compareSoftware(a, b) {
                if (a.name > b.name) {
                    return 1;
                }
                if (a.name < b.name) {
                    return -1;
                }
                return this.compareVersions(a.version, b.version) * -1;
            },
            compareVersions(v1, v2) {
                var vnum1 = 0, vnum2 = 0;

                for (var i = 0, j = 0; (i < v1.length || j < v2.length);) {
                    while (i < v1.length && v1[i] != '.') {
                        vnum1 = vnum1 * 10 + (v1[i] - '0');
                        i++;
                    }

                    while (j < v2.length && v2[j] != '.') {
                        vnum2 = vnum2 * 10 + (v2[j] - '0');
                        j++;
                    }

                    if (vnum1 > vnum2)
                        return 1;
                    if (vnum2 > vnum1)
                        return -1;

                    vnum1 = vnum2 = 0;
                    i++;
                    j++;
                }
                return 0;
            },
        },
    });
</script>

<style scoped>
    h2 {
        padding-bottom: 10px;
    }

    th, h2, h3, h4 {
        font-weight: bold;
    }

    tr:nth-child(even) {
        background: #F2F2F2;
    }

    tr:nth-child(odd) {
        background: #FFF;
    }

    th, td {
        padding-left: .5rem;
        padding-right: .5rem;
    }

    .weather-component {
        text-align: left;
    }

    table {
        margin-left: auto;
        margin-right: auto;
    }

    .display-section {
        display: flex;
        flex-direction: row;
    }

    .text-column {
        width: 60%;
    }

    .input-row {
        display: flex;
        flex-direction: row;
        width: 100%;
        max-width: 285px;
    }

    .search-box {
        position: relative;
        cursor: text;
        font-size: 14px;
        line-height: 20px;
        padding: 0 16px;
        height: 48px;
        background-color: #fff;
        border: 1px solid #d6d6e7;
        border-radius: 3px;
        color: rgb(35, 38, 59);
        box-shadow: inset 0 1px 4px 0 rgb(119 122 175 / 30%);
        overflow: hidden;
        transition: all 100ms ease-in-out;
        margin-right: 5px;
    }

    .serach-box:focus {
        border-color: #3c4fe0;
        box-shadow: 0 1px 0 0 rgb(35 38 59 / 5%);
    }

    .search-button {
        display: inline-block;
        outline: 0;
        cursor: pointer;
        text-align: center;
        border: 0;
        padding: 7px 16px;
        min-height: 36px;
        min-width: 36px;
        color: #ffffff;
        background: #008060;
        border-radius: 4px;
        font-weight: 500;
        font-size: 14px;
        box-shadow: rgba(0, 0, 0, 0.05) 0px 1px 0px 0px, rgba(0, 0, 0, 0.2) 0px -1px 0px 0px inset;
    }

        .search-button:hover {
            background: #006e52;
        }

    .results-column {
        width: 40%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding-top: 40px;
        height: 100%;
    }

    .results-content {
        display: flex;
        flex-direction: column;
        margin-left: 25%;
        margin-right: 25%;
    }

    .results-wrapper {
        border: solid;
        border-radius: 5px;
        overflow-y: auto;
        margin-top: 10px;
        max-height: 500px;
        max-width: 285px;
        width: 100%;
    }

    .results-column table {
        margin: 0px;
        width: 100%;
    }

    .loading {
        padding: 30px;
    }

    @media (max-width: 800px) {
        .display-section {
            flex-direction: column;
        }

        .text-column {
            width: 100%;
        }

        .results-column {
            width: 100%;
        }
    }
</style>