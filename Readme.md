# saikiran


services:
  itvdelab:
    image: itversity/itvdelab
    hostname: itvdelab
    ports:
      - "8888:8888"
    volumes:
      - "./itversity-material:/home/itversity/itversity-material"
      - "./data:/data"
    environment:
      SHELL: /bin/bash
    networks:
      - itvdelabnw
    depends_on:
      - "cluster_util_db"
  cluster_util_db:
    image: postgres:13
    ports:
      - "6432:5432"
    volumes:
      - ./cluster_util_db_scripts:/docker-entrypoint-initdb.d
    networks:
      - itvdelabnw
    environment:
      POSTGRES_PASSWORD: itversity
  itvdflab:
    build:
      context: .
      dockerfile: images/pythonsql/Dockerfile
    hostname: itvdflab
    ports:
      - "8888:8888"
    volumes:
      - "./itversity-material:/home/itversity/itversity-material"
      - "./data:/data"
    environment:
      SHELL: /bin/bash
    networks:
      - itvdelabnw
    depends_on:
      - "pg.itversity.com"
  pg.itversity.com:
    image: postgres:13
    ports:
      - "5432:5432"
    networks:
      - itvdelabnw
    environment:
      POSTGRES_PASSWORD: itversity
networks:
  itvdelabnw:
    name: itvdelabnw
,,,,,
