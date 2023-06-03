---
theme: uncover
marp: true
paginate: true
class: lead invert
footer: https://dylanj.xyz/graphql-talk
---

<style>
  footer {
    text-align: left;
    font-size: .30em;
  }
</style>

<style scoped>
  h1 {
    text-shadow: #000 1px 0 10px;
  }
</style>

# Unlocking Your APIs With GraphQL

![bg contain 40%](assets/graphql.svg)

<!-- Work at Qoria formally FamilyZone -->
<!-- Our experience adopting GraphQl, why we felt it was a good idea, how it went, and what we learnt from the experience -->

---

# REST is full of lies

---

![bg contain 90%](assets/rest_bffs.excalidraw.svg)

<!-- Using starwars as example data as that matches the GraphQL official documentation -->

---

![bg contain 60%](assets/rest_bffs_part2.excalidraw.svg)

---

![bg contain 60%](assets/rest_bffs_part3.excalidraw.svg)

---

## Problems?

* N+1
* Overfetching & Underfetching
* Server / Client Coupling

---

The Creation of GraphQL
![bg contain right:66% 70%](./assets/documentary_qr_code.svg)

---

Overfetching Underfetching
![bg contain right:66% 80%](./assets/overfetching.gif)

---

Hierarchical Data
![bg contain right:66% 80%](./assets/heriachal_data.gif)

---

## Hierarchical Data in Rest

* `GET /hero/{id} `
* `GET /hero/{id}/friends`
* `GET /hero/{id}/spaceships`
* `GET /hero`
* `GET /movie/{id}/hero`
* `GET /movie/{id}/hero/{friends?, spaceships?}`

---

Versionless Schema
![bg contain right:66% 80%](./assets/versionless.gif)

---

Strongly Typed Schema
![bg contain right:66% 80%](./assets/strongly_typed.gif)

---

DevX Tooling
![bg contain right:70% 90%](./assets/developer_tooling.gif)

---

![bg contain 75%](./assets/DiffusionOfInnovation.png)

---

![bg contain](./assets/invasion_graphql.png)


---
![bg contain 110%](./assets/codegen_flow1.88e173ec.png)

---

![bg contain 90%](./assets/graphql_advocate.png)

---

Ok, but how do I build it?

---

<style scoped>
  p {
    display: flex;
    justify-content: space-evenly;
    margin: 1em;
  }
</style>

It's dangerous to go alone, take these.

![width:400px](./assets/GitLab_logo.svg.png)
![width:400px](./assets/relay-logo.png)

---
<style scoped>
  section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1em;
    align-content: center;
  }

  marp-pre {
    margin: 0px
  }
</style>

```gql
# Schema

type Query {
  hero(episode: Episode): Character
}

type Character {
  name: String!
  appearsIn: [Episode]!
  friends: [Character]!
}
```

```js
// JS Code

export const resolvers = {
  Query: {
    hero: (episode) => { ... },
  },
  Character: {
    friends: (parent) => { ... },
  }
}
```

---

N+1? Dataloaders.

---

![contain](./assets/dataloaders_1.png)

---

![bg contain](./assets/dataloaders_2.png)

---

Pagination (Relay)

![width:1100px](assets/relay_pagination.png)

---

Choosing your page size matters a lot

---

![bg contain 75%](assets/Distribution%20Function%20of%20Recursive%20Subgroups.svg)

---

![bg contain 75%](assets/Distribution%20Function%20of%20Recursive%20Subgroups%20(Log).svg)

---

![bg contain 75%](assets/Cumulative%20Distribution%20Function%20of%20Recursive%20Subgroups.svg)

---
Nested Pagination?
```gql
query ($cursor:ID!, $nestedCursor:ID) {
  hero {
    name
    friendsConnection(first: 10, after: $cursor) {
      pageInfo {
        endCursor
        hasNextPage
      }
      edges {
        node {
          name
          friendsConnection(first: 10, after: $nestedCursor) {
            pageInfo {
              endCursor
              hasNextPage
            }
            edges {
              node {
                name
              }
            }
          }
        }
      }
    }
  }
}
```

---

<style scoped>
  section {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
  p {
    grid-column: 1 / span 2;
  }
</style>

Query Complexity

```gql
{
  heroes(first: 100) {
    friends(first: 100) {
      friends(first: 100) {
        friends(first: 100) {
          name
        }
      }
    }
  }
}
```

* Limit Nesting Depth
* Limit Overall Complexity

---

## Observability

Demo Time

---

## Next Steps: Federation

---

![bg contain 90%](assets/federation.png)

---

![bg contain 85%](assets/federation_detailed.png)

---

<style scoped>
  section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: auto auto 1fr;
    grid-auto-flow: column;
    align-items: start;
  }
  h1 {
    grid-column: span 2;
  }
  ul {
    align-self: start;
  }
</style>

# Summary

### For
* Lots of clients
* Lots of query parameters
* Code generation

### Against
* Requires rework
* Additional Complexity
* BFFS

---

# Thanks

- Source Code: [github.com/dylanrjohnston/graphql-talk](https://github.com/dylanrjohnston/graphql-talk)
- Slides: [dylanj.xyz/graphql-talk](https://dylanj.xyz/graphql-talk)