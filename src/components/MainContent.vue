<template>
    <h2 class="content-title">{{ content.label }}</h2>
    <component :is="targetComponent"></component>
</template>
<script>
import { defineAsyncComponent } from 'vue';
import { objectExpression } from '@babel/types';

export default {
    name: "MainContent",
    props: {
        content: {
            type:objectExpression,
            default() {
                return {
                    label: "TEST Menu",
                    path: "Test",
                    index: "TestMenu",
                };
            },
        },
    },
    date() {
        return {
            targetComponent: null,
        }
    },
    watch: {
        content: {
            deep: true,
            handler: function(newValue){
                this.targetComponent = defineAsyncComponent(() =>
                import(`@/components/${newValue.path}/${newValue.index}`)
                );
            }
        }
    }
};
</script>
<style>
</style>