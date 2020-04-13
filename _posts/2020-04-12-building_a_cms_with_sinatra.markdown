---
layout: post
title:      "Building a CMS with Sinatra"
date:       2020-04-12 22:16:58 -0400
permalink:  building_a_cms_with_sinatra
---


For my second Flatiron portfolio project, I decided to build a simple app to help users store information about their sexual partners. Its current form is very bare bones, but I hope to eventually build it out to not only be useful but also be designed with care that will promote sexual health. 

As it stands, the two models in the app are Users and Entries. A user `has_many :entries` and an entry `belongs_to :user`.  If I were to build it out more, I would probably add a Partners class and reassociate the models such that a user `has_many :partners` as well. 

Since the information being stored in this app is highly sensitive and private, I took care to authenticate the user session at every step. Users are unable to view other user profiles, let alone other users' entries. Both my erb files and controllers ensure this, by using methods that live in my Helper class. 

My Helper class contains two class methods: `self.is_logged_in?(session)` and `self.current_user(session)`. The first method verifies that there is in fact a user logged in: `session[:user_id] ? true : false`. The second method "identifies" the user: `User.find_by(id: session[:user_id])`. 

If, for example, a user is logged in and tries to view an entry that is not theirs, they will be redirected to their own home page. 
`if !Helpers.is_logged_in?(session) || !@entry || @entry.user !=Helpers.current_user(session)
            redirect '/'` 
						


