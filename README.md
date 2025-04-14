FROM mattermost/focalboard

EXPOSE 8000

CMD ["./focalboard-server", "--config", "./config.json"]
