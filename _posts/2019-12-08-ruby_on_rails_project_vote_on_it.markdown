---
layout: post
title:      "Ruby on rails project: Vote On It"
date:       2019-12-08 19:46:27 -0500
permalink:  ruby_on_rails_project_vote_on_it
---


Github Repo: [https://github.com/dkintop/vote-on-it](http://)

Project Demo Video: https://www.youtube.com/watch?v=IMP8_G2Pu6k

For my Ruby on Rails Project I decided to create an application that users could you to post questions or subjects that other users could then view and vote on by selecting one of two options. This Project was a lot of fun and I learned a lot while making it and overall things went really well with very few issues during the process. I did run into one issue while attempting to add a feature that would only allow a user to cast a vote on a subject that they have not yet voted on, and luckily I captured this whole process on video and I was then able to rewatch the video and after a minute of research discovered what the problem was. The video of me attempting to solve this problem can be found here: [https://www.youtube.com/watch?v=aomrVriZe-8](http://)(I apologize for the mumbling and rambling at times, I hadn't planned to use this video for anything until now). This feature took me longer to add than expected and I realize the video is much longer than most people are willing to sit through, so I will do my best to sum it up for you.

I began by figuring out the logic behind what I was trying to accomplish and figured this out pretty quickly, deciding that I would first check if the subjects users (which would refer to the users who have voted on the subject due to the way our relationships are set up, with the votes table being the joint table between users and subjects) included the current user before saving the vote to the database and eventually came up with this solution:

```
 def create 
       @vote = Vote.new(vote_params)
       
        if @vote.subject.users.include?(current_user)
            flash[:error] = "You already voted on this subject."
						render :new
        end
        
        if @vote.save
            redirect_to subject_votes_path(@vote.subject_id)
       else
            @subject = Subject.find_by_id(vote_params[:subject_id])
            render :new
       end
    end
```

at this point I thought I had nailed it, but also knew that there was a high probability of there being an error since, well, there almost always is at first when I attempt to insert logic into a controller action. So I ran it and got back the all to familiar "NoMethodError in Votes#create" with the line "<%=@subject.title%> in the view highlited. I realized at this point that it was attempting to render our new vote page without knowing at this point what @subject was, since the re render cleared that insance variable from memory. So I decided to try a redirect_to new_subject_vote_path(@vote.subject) to solve that issue as opposed to re rendering the :new page. this solved the initial problem, and now our create action looked like this:

```
def create 
       @vote = Vote.new(vote_params)
       
        if @vote.subject.users.include?(current_user)
            flash[:error] = "You already voted on this subject."
						redirect_to new_subject_vote_path(@vote.subject)
        end
        
        if @vote.save
            redirect_to subject_votes_path(@vote.subject_id)
       else
            @subject = Subject.find_by_id(vote_params[:subject_id])
            render :new
       end
    end
```

So now we have solved the error about our view not knowing what @subject was, but now I was getting the error "AbstractController::DoubleRenderError in VotesController#create." This was an error I had not seen before, but lucky for me Rails provides excellent error messages that lead me to the root of the problem. the error description read "Render and/or redirect were called multiple times in this action. Please note that you may only call render OR redirect, and at most once per action. Also note that neither redirect nor render terminate execution of the action, so if you want to exit an action after redirecting, you need to do something like "redirect_to(...) and return". At this point my brain hurt and the gears were starting to grind a bit, but I realized that what this error message was telling me was that this action was attempting to redirect during the initial if statment and the following if statment, and that was a no no.

I realized I was going to have to change my logic to ensure that the action was only redirecting one time, so after some trial and error I eventually came up with this solution:

```
def create 
       @vote = Vote.new(vote_params)
       
        if !@vote.subject.users.include?(current_user)
            @vote.save
        else
            flash[:duplicate_vote_error] = "You already voted on this subject. click here to see votes."
        end    
        
        if @vote.id 
            redirect_to subject_votes_path(@vote.subject_id)
       else
            @subject = Subject.find_by_id(vote_params[:subject_id])
            render :new
       end
    end
```

Don't worry! I refactored this immediately so the controller action wasn't so cluttered. This solved my problem by ony redirecting or re rendering one time based on the conditional @vote.id,  which would return truthy or falsey based on if @vote saved in the previous if statement since id's are only assigned when the object is saved to the database. 


The lesson learned here is that a render or redirect does not terminate a controller action, and the action will continue to execute after a redirect or render. This is very useful to know so that I do not make this mistake again but I can also see how you could use this to your advantage to complete additional logic or tasks after a redirect if neccessary, although at the moment I can not think of a scenario where you would want or need to. 

To see this feature in action, watch the demo video linked at the top of this blog (it is much shorter than the video of me attemting this problem).




