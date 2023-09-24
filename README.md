name: Time

on:

  push:
    branches:
      - master
      
  schedule:
  - cron: "0 */4 * * *" # See https://crontab.guru/#0_7,9,11_*_*_1,3

jobs:
  auto_commit:
    runs-on: ubuntu-latest
    steps:
      - name: DEBUG 
        run: echo "::debug::Ref = ${{github.ref}}"
      - uses: actions/checkout@v3      
        with:
         persist-credentials: false
         fetch-depth: 0

      - name: Modify last update
        run: |
          d=`date '+%Y-%m-%dT%H:%M:%SZ'`
          echo $d > Time
          
      - name: Commit changes
        run: |
          git config --local user.email "betrdh@rambler.ru"
          git config --local user.name "Jubo3"
          git add -A
          
          arr[0]="chore(bot): 😂 Time"
          arr[1]="chore(bot): 😱 Time"
          arr[2]="chore(bot): 👿 Time"
          arr[3]="chore(bot): 💩 Time"
          arr[4]="chore(bot): 🙏 Time"
          arr[5]="chore(bot): 🙈 Time"
          arr[6]="chore(bot): 🐐 Time"
          arr[7]="chore(bot): 🤖 Time"
          arr[8]="chore(bot): 🟩 Time"
          arr[9]="chore(bot): 👻 Time"
          
          rand=$[$RANDOM % ${#arr[@]}]
          
          git commit -m "${arr[$rand]}"
          
      - name: GitHub Push
        uses: ad-m/github-push-action@v0.6.0
        with:
          directory: "."
          github_token: ${{ secrets.TOKEN }}
