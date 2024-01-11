# Mirror GitHub single repos/users/orgas/starred repos to a Gitea instance

**Inspiration for these scripts**
- [Proposal: Mirror an entire github organization into gitea organization](https://github.com/go-gitea/gitea/issues/8424)
- This is a fork of [juergenhoetzel/github2gitea-mirror](https://github.com/juergenhoetzel/github2gitea-mirror)
- The deletion of an organization which still contains repositories is not possible, yet:
> This organization still owns one or more repositories; delete or transfer them first.


## Usage

Currently, there are two scripts in this repository.
One is able to create mirrors for various GitHub entities and the other is capable of deleting an entire Gitea organization with all of its repositories.

### Mirroring users, organizations, repos

First, setup your environment variables like this:
- `ACCESS_TOKEN` is an application access token of your Gitea instance (always needed).
- `GITEA_URL` is the URL of your Gitea instance (without trailing slash!) (always needed).
- `GITHUB_TOKEN` is a personal access token for your GitHub account. It is needed in case you want to access private repos of your account/an organization.

```bash
export ACCESS_TOKEN=123
export GITEA_URL=https://gitea.example.com
export GITHUB_TOKEN=456
export GITEA_USER=789
```

There are four modes of operation, which the script can print out to the console:
```bash
./github2gitea-mirror -m

Usage: ./github2gitea-mirror
   -m, --mode {org,star,repo,user}     Mode to use; either mirror an organization or mirror all starred repositories.
   -o, --org $organization             GitHub organization to mirror and/or the target organization in Gitea.
   -u, --user $github_user             GitHub user to gather the starred repositories from.
   -v, --visibility {public,private}   Visibility for the created Gitea organization.
   -r, --repo $repo_url                GitHub URL of a single repo to create a mirror for.
```

- `--mode`
  - `org`: Mirror a complete oranization with all its public/private repositories.
  - `star`: Mirror all starred repositories by user `$github_user`.
  - `repo`: Mirror a single (public or private) repository.
  - `user`: Mirror a complete user with all its public/private repositories.

#### Examples

Mirror a complete GitHub organization:
```bash
./github2gitea-mirror -m org -o $myOrga -v public -u $myGitHubUser
```

Mirror all starred GitHub repos of user `$myGitHubUser`:
```bash
./github2gitea-mirror -m star -u $myGitHubUser -o $myGiteaOrga
```

Mirror a single GitHub repository:
```bash
./github2gitea-mirror -m repo https://github.com/maxkratz/github2gitea-mirror -u $myGitHubUser
```

Mirror a complete GitHub user:
```bash
./github2gitea-mirror -m user -u $myGitHubUser
```

#### TODOs
  - [x] Mirror a complete GitHub organization
  - [x] Mirror (single) private repos
  - [x] Mirror a complete GitHub user (with all public/private repos)

### Deleting Gitea organizations with all repositories

First, setup your environment variables like this:
- `ACCESS_TOKEN` is an application access token of your Gitea instance.
- `GITEA_URL` is the URL of your Gitea instance (without trailing slash!).

```bash
export ACCESS_TOKEN=123
export GITEA_URL=https://gitea.example.com
```

Use parameter `-o` to pass the Gitea organization which should get deleted to the script.

#### Examples

Delete the Gitea organization `$myGiteaOrg`:
```bash
./delete_gitea_org -o $myGiteaOrg
```


## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for more details.
