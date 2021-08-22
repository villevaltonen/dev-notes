## Go modules
```golang
// initialize a project with go modules
go mod init github.com/villevaltonen/<project-repo> 

// adds missing module requirements for imported packages and removes requirements on modules that aren't used anymore
go mod tidy

// list all modules
go list -m all
```