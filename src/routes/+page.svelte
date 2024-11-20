<script lang="ts">
    import { fade, fly, slide } from 'svelte/transition';
    import { onMount } from 'svelte';
    import { writable } from 'svelte/store';
    import { Button } from "$lib/components/ui/button";
    import { Card } from "$lib/components/ui/card";
    import { Input } from "$lib/components/ui/input";
    import { Switch } from "$lib/components/ui/switch";
    import { 
      LayoutGrid, 
      Calendar, 
      FileText, 
      Settings, 
      Bell,
      Search,
      Plus,
      X,
      Sun,
      Moon
    } from 'lucide-svelte';
    
    interface Metric {
      title: string;
      value: string;
      icon: string;
      color: string;
    }
  
    interface Notification {
      icon: string;
      text: string;
      time: string;
      color: string;
    }
  
    interface Action {
      text: string;
      color: string;
    }
  
    interface NavItem {
      icon: typeof LayoutGrid;
      active: boolean;
    }
  
    let mounted = false;
    let isDarkMode = false;
    let currentDate = new Date().toLocaleDateString('en-US', { day: 'numeric', month: 'long', year: 'numeric' });
    
    onMount(() => {
      mounted = true;
    });
  
    const showNotifications = writable(false);
  
    const toggleDarkMode = () => {
      isDarkMode = !isDarkMode;
      document.documentElement.classList.toggle('dark', isDarkMode);
    };
  
    const metrics: Metric[] = [
      {
        title: "Potential Monthly Profit",
        value: "$24,042,000",
        icon: "💰",
        color: "bg-rose-50 text-rose-500 dark:bg-rose-900 dark:text-rose-300"
      },
      {
        title: "Worker's Wage This Month",
        value: "$8,402,000",
        icon: "📊",
        color: "bg-blue-50 text-blue-500 dark:bg-blue-900 dark:text-blue-300"
      },
      {
        title: "Average Project Length",
        value: "2 weeks",
        icon: "📅",
        color: "bg-amber-50 text-amber-500 dark:bg-amber-900 dark:text-amber-300"
      },
      {
        title: "Average Income per Project",
        value: "$12,000",
        icon: "📈",
        color: "bg-emerald-50 text-emerald-500 dark:bg-emerald-900 dark:text-emerald-300"
      }
    ];
  
    const notifications: Notification[] = [
      {
        icon: "📱",
        text: "You've added new project recently with no deadline",
        time: "2m ago",
        color: "bg-amber-100 dark:bg-amber-800"
      },
      {
        icon: "📸",
        text: "Project owner Adam requested a refund",
        time: "5m ago",
        color: "bg-rose-100 dark:bg-rose-800"
      },
      {
        icon: "🎉",
        text: "Today it's Tim's anniversary!",
        time: "1h ago",
        color: "bg-blue-100 dark:bg-blue-800"
      }
    ];
  
    const actions: Action[] = [
      { text: 'Check Calendar', color: 'text-blue-500 dark:text-blue-400' },
      { text: 'Manage Wallet', color: 'text-amber-500 dark:text-amber-400' },
      { text: 'Manage Printers', color: 'text-rose-500 dark:text-rose-400' },
      { text: 'Manage Projects', color: 'text-blue-500 dark:text-blue-400' }
    ];
  
    const navItems: NavItem[] = [
      { icon: LayoutGrid, active: true },
      { icon: Calendar, active: false },
      { icon: FileText, active: false },
      { icon: Settings, active: false }
    ];
  </script>
  
  <div class="min-h-screen bg-[#F5F7FF] dark:bg-gray-900 flex flex-col md:flex-row transition-colors duration-300">
    <!-- Sidebar for desktop -->
    <nav class="hidden md:flex fixed left-0 top-0 h-full w-[72px] bg-white dark:bg-gray-800 shadow-sm flex-col items-center py-6 gap-8 z-10">
      <div class="w-8 h-8">
        <img src="/logo.svg" alt="Logo" class="w-full h-full" />
      </div>
      {#if mounted}
        <div class="flex flex-col gap-6" transition:fade>
          {#each navItems as { icon: Icon, active }, i}
            <button 
              class="p-3 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors duration-200 {active ? 'bg-gray-50 dark:bg-gray-700' : ''}"
              in:fly={{ y: 20, delay: i * 100 }}
            >
              <svelte:component this={Icon} class="w-5 h-5 text-gray-400 dark:text-gray-300" />
            </button>
          {/each}
        </div>
      {/if}
    </nav>
  
    <!-- Main Content -->
    <main class="flex-1 md:pl-[72px] pb-16 md:pb-0 min-h-screen">
      <div class="p-4 md:p-8">
        <!-- Header -->
        <header class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
          <div class="relative w-full md:w-auto">
            <Search class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400 dark:text-gray-300" />
            <Input 
              type="text" 
              placeholder="Search..." 
              class="pl-10 w-full md:w-64 rounded-full border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 dark:text-white"
            />
          </div>
          <div class="flex flex-col md:flex-row items-start md:items-center gap-4 md:gap-6 w-full md:w-auto">
            <span class="text-sm text-gray-600 dark:text-gray-400">{currentDate}</span>
            <div class="flex items-center gap-3 w-full md:w-auto">
              <Button variant="ghost" size="sm" class="gap-2 w-full md:w-auto justify-between" on:click={toggleDarkMode}>
                <span class="flex items-center gap-2">
                  {#if isDarkMode}
                    <Moon class="w-4 h-4" />
                    Night
                  {:else}
                    <Sun class="w-4 h-4" />
                    Day
                  {/if}
                </span>
                <Switch checked={isDarkMode} />
              </Button>
              <Button variant="ghost" size="sm" class="w-full md:w-auto" on:click={() => showNotifications.update(n => !n)}>
                <Bell class="w-4 h-4" />
              </Button>
              <Button variant="default" size="sm" class="bg-blue-600 dark:bg-blue-700 text-white rounded-lg px-4 py-2 flex items-center gap-2 w-full md:w-auto justify-center">
                <Plus class="w-4 h-4" />
                Create new
              </Button>
            </div>
            <div class="w-8 h-8 rounded-full bg-gray-200 dark:bg-gray-700 hidden md:block">
                <img src="https://i.pinimg.com/236x/ba/4d/e2/ba4de223a81032be322ceeebe6e05671.jpg" alt="" class="rounded-full w-full h-full object-cover object-center">
            </div>
          </div>
        </header>
  
        {#if mounted}
          <div>
            <h1 class="text-xl font-semibold mb-6 dark:text-white">Dashboard</h1>
            
            <!-- Welcome Card -->
            <Card class="p-6 md:p-8 mb-8 bg-white dark:bg-gray-800 rounded-2xl shadow-sm">
              <div class="flex flex-col md:flex-row justify-between items-center gap-6">
                <div>
                  <h2 class="text-2xl font-bold mb-2 dark:text-white">Hi, George!</h2>
                  <p class="text-gray-600 dark:text-gray-400 mb-6">What are we doing today?</p>
                  <div class="flex flex-wrap gap-4">
                    {#each actions as { text, color }}
                      <Button 
                        variant="outline" 
                        size="sm"
                        class="rounded-full border-gray-200 dark:border-gray-700 {color}"
                      >
                        {text}
                      </Button>
                    {/each}
                  </div>
                </div>
                <img 
                  src="/polar-bear.png" 
                  alt="Cute polar bear" 
                  class="w-32 h-32 md:w-40 md:h-40 object-contain"
                />
              </div>
            </Card>
  
            <!-- Metrics -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 md:gap-6">
              {#each metrics as { title, value, icon, color }}
                <Card 
                  class="p-4 md:p-6 bg-white dark:bg-gray-800 rounded-2xl shadow-sm"
                >
                  <div class="flex items-start justify-between">
                    <div>
                      <p class="text-sm text-gray-500 dark:text-gray-400">{title}</p>
                      <p class="text-lg md:text-xl font-bold mt-2 dark:text-white">{value}</p>
                    </div>
                    <div class="p-3 rounded-xl {color}">
                      <span class="text-xl">{icon}</span>
                    </div>
                  </div>
                </Card>
              {/each}
            </div>
          </div>
        {/if}
      </div>
    </main>
  
    <!-- Bottom Navbar for mobile -->
    <nav class="md:hidden fixed bottom-0 left-0 right-0 bg-white dark:bg-gray-800 shadow-lg flex justify-around items-center h-16 z-10">
      {#each navItems as { icon: Icon, active }}
        <button 
          class="p-3 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors duration-200 {active ? 'bg-gray-50 dark:bg-gray-700' : ''}"
        >
          <svelte:component this={Icon} class="w-5 h-5 text-gray-400 dark:text-gray-300" />
        </button>
      {/each}
    </nav>
  
    <!-- Notifications Panel -->
    {#if $showNotifications}
      <aside 
        class="fixed md:relative inset-0 bg-white dark:bg-gray-800 p-6 overflow-y-auto z-50 md:w-[320px] md:right-0 md:left-auto"
        transition:slide={{ duration: 300, axis: 'x' }}
      >
        <div class="flex justify-between items-center mb-6">
          <div class="flex items-center gap-2">
            <h2 class="font-semibold dark:text-white">Notifications</h2>
            <div class="w-2 h-2 rounded-full bg-blue-600"></div>
          </div>
          <Button variant="ghost" size="sm" on:click={() => showNotifications.set(false)}>
            <X class="w-4 h-4 dark:text-white" />
          </Button>
        </div>
        <div class="space-y-4">
          {#each notifications as { icon, text, time, color }}
            <div 
              class="flex gap-4 p-4 rounded-xl bg-gray-50 dark:bg-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 transition-colors duration-200"
              in:fly={{ x: 20, delay: 700 }}
            >
              <div class={`w-10 h-10 ${color} rounded-lg flex items-center justify-center`}>
                <span class="text-lg">{icon}</span>
              </div>
              <div>
                <p class="text-sm dark:text-white">{text}</p>
                <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">{time}</p>
              </div>
            </div>
          {/each}
        </div>
      </aside>
    {/if}
  </div>
  
  <style>
    :global(body) {
      background-color: #F5F7FF;
    }
    :global(body.dark) {
      background-color: #1a202c;
    }
  </style>