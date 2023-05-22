<script>
import axios from 'axios'

export default {
    async setup() {
        const res = await axios.get('https://api.github.com/users/zjy4fun/starred')
        const starredListFullNames = res.data.map(item => {
            return {
                full_name: item.full_name,
            }
        })

        console.log(starredListFullNames)

        const issuePrefix = 'https://api.github.com/repos'
        const issueList = await Promise.all(starredListFullNames.map(async item => {
            const res = await axios.get(`${issuePrefix}/${item.full_name}/issues`)
            let issues = res.data.map(issue => {
                return {
                    title: issue.title,
                    html_url: issue.html_url,
                    labels: issue.labels,
                }
            })
            return {
                full_name: item.full_name,
                issues
            }
        }))
        console.log(issueList)
        return {
            issueList
        }
    }
}

</script>

<template>
    <h1>issue from starred list</h1>
    <ul>
        <li v-for="(item, index) in issueList" :key="index">
            <h3>{{ item.full_name }}</h3>
            <ul>
                <li v-for="(issue, index) in item.issues" :key="index">
                    <a :href="issue.html_url">{{ issue.title }}</a>
                    <span v-for="(label, index) in issue.labels" :key="index">
                        [{{ label.name }}]
                    </span>
                </li>
            </ul>
        </li>
    </ul>
</template>

<style scoped>

</style>
