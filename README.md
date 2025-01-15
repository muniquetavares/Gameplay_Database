## 💻 Gameplay Database

NoSQL database created using the following tools: MongoDB, Mockaroo, MySQL Workbench; and the following languages: Ruby and JavaScript.

Link to the server may be provided upon request due to security reasons. As reference, I have uploaded a json file of the database content, and a jason sample of a single document.

🚀 In this project, my group and I developed a noSQL database to help a company track the minutes played during gaming sessions. This database is designed to monitor user activity, device usage, gaming trends, and session reasons, providing valuable insights and simplifying the generation of detailed reports.

I was responsible for designing the star schema, generating fake data using the Mockaroo website, and migrating the data to the MongoDB database.

📌 Suggested queries using javascript:

1. What is the average session duration by game genre?

`db.games_session.aggregate([{
    $group: {
        _id: "$game.genre",
        avgSessionDuration: { $avg: "$total_minutes" }
    }
},
{
    $sort: { avgSessionDuration: -1 }
}])`


2. What is the distribution of operating systems among users who play each game genre?

`db.games_session.aggregate([
    {
        $group: {
            _id: {
                genre: "$game.genre",
                operating_system: "$device.operating_system"
            },
            count: { $sum: 1 }
        }
    },
    {
        $sort: { "_id.genre": 1, "_id.operating_system": 1 }
    }
]);`

3. What are the most common reasons for starting game sessions by game genre?

`db.games_session.aggregate([{
    $group: {
        _id: {
            genre: "$game.genre",
            reason: "$session_start_reason.reason"
        },
        count: { $sum: 1 }
    }
},
{
    $sort: { count: -1 }
},
{
    $group: {
        _id: "$_id.genre",
        mostCommonReason: { $first: "$_id.reason" },
        reasonCount: { $first: "$count" }
    }
}
])`

