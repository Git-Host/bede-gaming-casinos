<style lang="scss">
    #map {
        // max-height: 400px !important;
    }
</style>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-10 col-md-offset-1">
                <div class="panel panel-default">
                    <div class="panel-heading">Listed Casino&rsquo;s</div>

                    <div class="panel-body">
                        Below is a list of all our casino&rsquo;s, sorted by the closest to your current location!
                    </div>

                    <ul class="list-group">
                        <a
                            v-for="casino in casinos"
                            class="list-group-item"
                            v-link="{
                                name: 'casino',
                                params: {
                                    id: casino.id
                                }
                            }"
                        >
                            {{ casino.name }} ({{ parseFloat(casino.distance).formatMoney('') }} miles from your current location)
                        </a>
                    </ul>
                </div>
            </div>
        </div>

        <div>
            <map :casinos.sync="casinos"></map>
        </div>
    </div>
</template>

<script>
    export default {
        data: function() {
            return {
                center: {
                    lat: 54.9736486,
                    lng: -1.6226131
                },
                zoom: 13,
                mapBounds: {},
                gridSize: 50,
                mapType: 'terrain',
                markers: [],
                markersEven: false,
                drag: 0,
                mapClickedCount: 0,
                mapStyle: 'green',
                circleBounds: {},
                displayCircle: false,
                displayRectangle: false,
                casinos: {},
            };
        },
        components: {
            map: require('./map.vue'),
        },
        route: {
            data: function(transition) {
                var self = this;

                // get(url, [data], [options])
                return this.$http.get("casinos")
                    .then(response => {
                        return {
                            casinos: response.data,
                        };
                    }, error_response => {
                        console.log('error');
                        console.log(error_response);
                    });
            },
        }
    }
</script>
